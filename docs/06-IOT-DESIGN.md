# IOT SYSTEM DESIGN - LED LOCATION FINDER

## 🎯 MỤC TIÊU IOT SYSTEM

Khi người dùng tìm sách trên website:
1. Hệ thống gửi lệnh đến thiết bị IoT
2. LED trên nóc kệ sáng (LED lớn)
3. Dải LED ở ngăn sách cụ thể sáng
4. Sinh viên tìm thấy sách và nhấn nút
5. Đèn tắt và gửi feedback về website

---

## 🔌 HARDWARE COMPONENTS

### Option 1: ESP8266 (Khuyến nghị - Rẻ nhất)

**Tính năng**:
- WiFi built-in
- Giá rẻ (~50k/board)
- Dễ lập trình (Arduino IDE)
- 11 GPIO pins
- 3.3V logic

**Thích hợp cho**: Dự án ngân sách thấp, đơn giản

### Option 2: ESP32 (Tốt hơn nhưng đắt hơn)

**Tính năng**:
- WiFi + Bluetooth
- Giá ~80-100k/board
- Mạnh hơn ESP8266
- Nhiều GPIO hơn (34 pins)
- 3.3V logic

**Thích hợp cho**: Nếu cần tính năng phức tạp hơn trong tương lai

---

## 📦 BILL OF MATERIALS (BOM)

### Cho 10 kệ sách:

| Item | Specification | Quantity | Unit Price | Total | Notes |
|------|--------------|----------|------------|-------|-------|
| **ESP8266 NodeMCU** | WiFi board | 10 | 50,000đ | 500,000đ | Main controller |
| **LED Bulb 12V** | 3W, white | 10 | 10,000đ | 100,000đ | Top of shelf |
| **LED Strip 12V** | 5050 SMD, 1m | 10 | 50,000đ | 500,000đ | Book row |
| **Push Button** | Momentary switch | 10 | 5,000đ | 50,000đ | User feedback |
| **Relay Module** | 5V 2-channel | 10 | 15,000đ | 150,000đ | Control 12V LEDs |
| **Power Supply 12V** | 2A adapter | 5 | 50,000đ | 250,000đ | 2 shelves/adapter |
| **Resistors** | 10kΩ | 20 | 500đ | 10,000đ | Pull-up/down |
| **Jumper Wires** | Male-Female | 5 packs | 10,000đ | 50,000đ | Connections |
| **Breadboard** | Small | 10 | 10,000đ | 100,000đ | Prototyping |
| **USB Cables** | Micro USB | 10 | 10,000đ | 100,000đ | Programming |
| **Enclosure Box** | Plastic box | 10 | 15,000đ | 150,000đ | Protection |
| **Misc** | Wires, solder | - | - | 50,000đ | Additional |
| **TOTAL** | | | | **2,010,000đ** | ~2tr VNĐ |

**Lưu ý**: Giá có thể thay đổi, mua số lượng lớn có thể giảm giá

---

## 🔧 CIRCUIT DIAGRAM

### Wiring for Each Shelf Unit:

```
ESP8266 NodeMCU          Relay Module           12V LEDs
┌───────────┐           ┌──────────┐
│           │           │          │
│  D1 ──────┼──────────►│ IN1      │───► LED Bulb (Top)
│           │           │          │     12V 3W
│  D2 ──────┼──────────►│ IN2      │───► LED Strip (Row)
│           │           │          │     12V 5050
│  D5 ◄─────┼───────    │  VCC ◄───┼──── 5V (from NodeMCU)
│           │    │      │  GND ─────┼──── GND
│  3V3 ─────┼────┴──┐   └──────────┘
│  GND ─────┼───────┤
│           │       │   12V Power Supply
│  VIN ◄────┼───────┘   ┌──────────┐
└───────────┘           │  12V 2A  │───► Common 12V
                        │          │───► Common GND
   Push Button          └──────────┘
      │
   ┌──┴──┐
   │     │
   │  NC │
   │     │
   └──┬──┘
      │
      └───────► D5 (with 10kΩ pull-up)
```

### Component Connections:

**ESP8266 Pins**:
- `D1 (GPIO5)` → Relay IN1 (LED Bulb control)
- `D2 (GPIO4)` → Relay IN2 (LED Strip control)
- `D5 (GPIO14)` → Push Button (with pull-up resistor)
- `3V3` → Button pull-up resistor
- `VIN` → 5V from power adapter (if separate)
- `GND` → Common ground

**Relay Module**:
- `VCC` → 5V from ESP8266
- `GND` → Common ground
- `IN1` → D1 (LED Bulb)
- `IN2` → D2 (LED Strip)
- `COM` → 12V+ from power supply
- `NO1` → LED Bulb (+)
- `NO2` → LED Strip (+)

**LEDs**:
- LED Bulb (-) → GND
- LED Strip (-) → GND

---

## 💻 FIRMWARE CODE

### Arduino Sketch for ESP8266

```cpp
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClient.h>
#include <ArduinoJson.h>

// WiFi credentials
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// Server endpoint
const char* serverUrl = "http://your-api-url.com/api/iot";

// Device configuration
const char* deviceId = "SHELF_01";  // Unique for each device
const char* shelfCode = "A1";       // Shelf location

// Pin definitions
const int LED_BULB_PIN = D1;        // GPIO5 - Top LED
const int LED_STRIP_PIN = D2;       // GPIO4 - Row LED
const int BUTTON_PIN = D5;          // GPIO14 - Push button

// State variables
bool ledOn = false;
int lastButtonState = HIGH;
unsigned long lastCheckTime = 0;
const unsigned long CHECK_INTERVAL = 5000; // Check every 5 seconds

WiFiClient client;

void setup() {
  Serial.begin(115200);
  
  // Initialize pins
  pinMode(LED_BULB_PIN, OUTPUT);
  pinMode(LED_STRIP_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Turn off LEDs initially
  digitalWrite(LED_BULB_PIN, LOW);
  digitalWrite(LED_STRIP_PIN, LOW);
  
  // Connect to WiFi
  connectWiFi();
  
  // Register device with server
  registerDevice();
  
  Serial.println("Device ready!");
}

void loop() {
  // Check WiFi connection
  if (WiFi.status() != WL_CONNECTED) {
    connectWiFi();
  }
  
  // Check for commands from server
  if (millis() - lastCheckTime >= CHECK_INTERVAL) {
    checkForCommands();
    lastCheckTime = millis();
  }
  
  // Check button press
  int buttonState = digitalRead(BUTTON_PIN);
  if (buttonState == LOW && lastButtonState == HIGH) {
    delay(50); // Debounce
    if (digitalRead(BUTTON_PIN) == LOW) {
      handleButtonPress();
    }
  }
  lastButtonState = buttonState;
  
  delay(10);
}

void connectWiFi() {
  Serial.print("Connecting to WiFi...");
  WiFi.begin(ssid, password);
  
  int attempts = 0;
  while (WiFi.status() != WL_CONNECTED && attempts < 20) {
    delay(500);
    Serial.print(".");
    attempts++;
  }
  
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nConnected!");
    Serial.print("IP: ");
    Serial.println(WiFi.localIP());
  } else {
    Serial.println("\nFailed to connect. Retrying...");
    delay(5000);
    connectWiFi();
  }
}

void registerDevice() {
  if (WiFi.status() != WL_CONNECTED) return;
  
  HTTPClient http;
  String url = String(serverUrl) + "/devices/register/";
  
  http.begin(client, url);
  http.addHeader("Content-Type", "application/json");
  
  // Create JSON payload
  StaticJsonDocument<200> doc;
  doc["device_code"] = deviceId;
  doc["name"] = "Shelf " + String(shelfCode);
  doc["location_shelf"] = shelfCode;
  doc["ip_address"] = WiFi.localIP().toString();
  doc["mac_address"] = WiFi.macAddress();
  
  String payload;
  serializeJson(doc, payload);
  
  int httpCode = http.POST(payload);
  
  if (httpCode > 0) {
    String response = http.getString();
    Serial.println("Device registered: " + response);
  } else {
    Serial.println("Registration failed: " + String(httpCode));
  }
  
  http.end();
}

void checkForCommands() {
  if (WiFi.status() != WL_CONNECTED) return;
  
  HTTPClient http;
  String url = String(serverUrl) + "/commands/pending/?device_code=" + String(deviceId);
  
  http.begin(client, url);
  int httpCode = http.GET();
  
  if (httpCode == 200) {
    String response = http.getString();
    
    // Parse JSON response
    StaticJsonDocument<512> doc;
    DeserializationError error = deserializeJson(doc, response);
    
    if (!error && doc.containsKey("command")) {
      String command = doc["command"];
      int commandId = doc["id"];
      
      if (command == "LED_ON") {
        turnOnLEDs();
        sendCommandFeedback(commandId, "executed");
      } else if (command == "LED_OFF") {
        turnOffLEDs();
        sendCommandFeedback(commandId, "executed");
      }
    }
  }
  
  http.end();
}

void turnOnLEDs() {
  digitalWrite(LED_BULB_PIN, HIGH);
  digitalWrite(LED_STRIP_PIN, HIGH);
  ledOn = true;
  Serial.println("LEDs ON");
}

void turnOffLEDs() {
  digitalWrite(LED_BULB_PIN, LOW);
  digitalWrite(LED_STRIP_PIN, LOW);
  ledOn = false;
  Serial.println("LEDs OFF");
}

void handleButtonPress() {
  if (ledOn) {
    turnOffLEDs();
    sendButtonFeedback();
    Serial.println("Button pressed - LEDs turned off");
  }
}

void sendCommandFeedback(int commandId, String status) {
  if (WiFi.status() != WL_CONNECTED) return;
  
  HTTPClient http;
  String url = String(serverUrl) + "/commands/" + String(commandId) + "/feedback/";
  
  http.begin(client, url);
  http.addHeader("Content-Type", "application/json");
  
  StaticJsonDocument<200> doc;
  doc["status"] = status;
  doc["feedback"] = "Command executed successfully";
  
  String payload;
  serializeJson(doc, payload);
  
  int httpCode = http.POST(payload);
  Serial.println("Feedback sent: " + String(httpCode));
  
  http.end();
}

void sendButtonFeedback() {
  if (WiFi.status() != WL_CONNECTED) return;
  
  HTTPClient http;
  String url = String(serverUrl) + "/devices/button-pressed/";
  
  http.begin(client, url);
  http.addHeader("Content-Type", "application/json");
  
  StaticJsonDocument<200> doc;
  doc["device_code"] = deviceId;
  doc["action"] = "button_pressed";
  doc["timestamp"] = millis();
  
  String payload;
  serializeJson(doc, payload);
  
  int httpCode = http.POST(payload);
  Serial.println("Button feedback sent: " + String(httpCode));
  
  http.end();
}
```

---

## 🌐 BACKEND API ENDPOINTS

### IoT Device Management

#### 1. Register Device
```http
POST /api/iot/devices/register/
Content-Type: application/json

{
  "device_code": "SHELF_01",
  "name": "Shelf A1",
  "location_shelf": "A1",
  "ip_address": "192.168.1.100",
  "mac_address": "AA:BB:CC:DD:EE:FF"
}

Response:
{
  "id": 1,
  "device_code": "SHELF_01",
  "status": "active"
}
```

#### 2. Get Pending Commands
```http
GET /api/iot/commands/pending/?device_code=SHELF_01

Response:
{
  "id": 123,
  "command": "LED_ON",
  "book_id": 456,
  "created_at": "2025-11-02T10:00:00Z"
}
```

#### 3. Send Command Feedback
```http
POST /api/iot/commands/123/feedback/
Content-Type: application/json

{
  "status": "executed",
  "feedback": "Command executed successfully"
}

Response:
{
  "message": "Feedback received"
}
```

#### 4. Button Pressed Feedback
```http
POST /api/iot/devices/button-pressed/
Content-Type: application/json

{
  "device_code": "SHELF_01",
  "action": "button_pressed",
  "timestamp": 1234567890
}

Response:
{
  "message": "Feedback received",
  "led_status": "off"
}
```

### Book Location Finder

#### 5. Trigger LED for Book
```http
POST /api/books/{book_id}/locate/
Authorization: Bearer <token>

Response:
{
  "book_id": 456,
  "shelf": "A1",
  "row": "3",
  "device_code": "SHELF_01",
  "command_id": 123,
  "status": "sent"
}
```

#### 6. Turn Off LED
```http
POST /api/iot/commands/{command_id}/turn-off/
Authorization: Bearer <token>

Response:
{
  "command_id": 123,
  "status": "off_command_sent"
}
```

---

## 📱 FRONTEND INTEGRATION

### Book Detail Page - Locate Button

```typescript
// BookDetailPage.tsx

import { useState } from 'react';
import { Button, Spinner, Alert } from 'react-bootstrap';
import { FaMapMarkerAlt } from 'react-icons/fa';

const BookDetailPage = () => {
  const [locating, setLocating] = useState(false);
  const [ledActive, setLedActive] = useState(false);
  const [commandId, setCommandId] = useState<number | null>(null);

  const handleLocateBook = async () => {
    setLocating(true);
    try {
      const response = await axios.post(`/api/books/${bookId}/locate/`);
      setCommandId(response.data.command_id);
      setLedActive(true);
      toast.success('Đèn LED đã được bật! Hãy đến kệ sách để tìm.');
    } catch (error) {
      toast.error('Không thể bật đèn LED. Vui lòng thử lại.');
    } finally {
      setLocating(false);
    }
  };

  const handleTurnOffLED = async () => {
    if (!commandId) return;
    
    try {
      await axios.post(`/api/iot/commands/${commandId}/turn-off/`);
      setLedActive(false);
      setCommandId(null);
      toast.success('Đèn LED đã tắt.');
    } catch (error) {
      toast.error('Không thể tắt đèn LED.');
    }
  };

  return (
    <div>
      {/* Book details */}
      
      <div className="book-location mt-3">
        <h5>Vị trí sách</h5>
        <p>
          <strong>Kệ:</strong> {book.location_shelf} | 
          <strong> Ngăn:</strong> {book.location_row}
        </p>
        
        {!ledActive ? (
          <Button 
            variant="primary" 
            onClick={handleLocateBook}
            disabled={locating}
          >
            {locating ? (
              <>
                <Spinner size="sm" /> Đang bật đèn...
              </>
            ) : (
              <>
                <FaMapMarkerAlt /> Tìm vị trí sách
              </>
            )}
          </Button>
        ) : (
          <>
            <Alert variant="success">
              Đèn LED đang sáng tại kệ {book.location_shelf}!
              <br />
              Nhấn nút trên kệ khi đã tìm thấy sách.
            </Alert>
            <Button 
              variant="danger" 
              onClick={handleTurnOffLED}
            >
              Tắt đèn
            </Button>
          </>
        )}
      </div>
    </div>
  );
};
```

---

## 🛠️ INSTALLATION & SETUP GUIDE

### Step 1: Hardware Assembly

1. **Prepare Components**:
   - Unpack all components
   - Test LEDs with power supply
   - Test ESP8266 with USB

2. **Connect Circuit**:
   - Follow circuit diagram
   - Use breadboard for prototyping
   - Double-check connections
   - Test each component individually

3. **Mount Components**:
   - Mount ESP8266 in enclosure
   - Mount relay module
   - Install LED bulb on top of shelf
   - Install LED strip in book row
   - Install button in accessible location

### Step 2: Firmware Programming

1. **Install Arduino IDE**:
   ```bash
   Download from: https://www.arduino.cc/en/software
   ```

2. **Install ESP8266 Board**:
   - Open Arduino IDE
   - Go to: File → Preferences
   - Add URL: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
   - Go to: Tools → Board → Boards Manager
   - Search and install "esp8266"

3. **Install Libraries**:
   - Go to: Sketch → Include Library → Manage Libraries
   - Install: ArduinoJson (by Benoit Blanchon)

4. **Configure Code**:
   - Open firmware code
   - Update WiFi credentials
   - Update server URL
   - Update device ID and shelf code

5. **Upload Code**:
   - Connect ESP8266 via USB
   - Select: Tools → Board → NodeMCU 1.0
   - Select: Tools → Port → (Your COM port)
   - Click: Upload
   - Wait for "Done uploading"
   - Open Serial Monitor (115200 baud)

### Step 3: Backend Setup

1. **Database Migrations**:
   ```bash
   python manage.py makemigrations iot
   python manage.py migrate iot
   ```

2. **Test API**:
   - Use Postman to test endpoints
   - Register device manually
   - Send test commands

### Step 4: Testing

1. **Test WiFi Connection**:
   - Power on ESP8266
   - Check Serial Monitor
   - Verify IP address obtained

2. **Test Device Registration**:
   - Check device registered in admin panel
   - Verify device status

3. **Test LED Control**:
   - Send LED_ON command from API
   - Verify LEDs turn on
   - Press button
   - Verify LEDs turn off
   - Check feedback received

4. **Test from Frontend**:
   - Navigate to book detail page
   - Click "Locate Book"
   - Verify LEDs turn on
   - Press button on shelf
   - Verify LEDs turn off

---

## 📊 POWER CONSUMPTION

### Per Shelf Unit:

| Component | Voltage | Current | Power |
|-----------|---------|---------|-------|
| ESP8266 | 3.3V | 80mA | 0.26W |
| Relay Module | 5V | 15mA | 0.08W |
| LED Bulb | 12V | 250mA | 3W |
| LED Strip | 12V | 300mA | 3.6W |
| **Total (LED ON)** | | | **~7W** |
| **Total (Standby)** | | | **~0.5W** |

### For 10 Shelves:
- **Standby**: 10 × 0.5W = 5W (minimal)
- **All LEDs ON**: 10 × 7W = 70W (worst case, unlikely)
- **Typical**: 1-2 shelves active = 10-15W

**Monthly Cost** (assuming 8 hours/day, 5W average, 3,000đ/kWh):
- 5W × 8h × 30 days = 1.2 kWh
- 1.2 kWh × 3,000đ = **3,600đ/month** (very cheap!)

---

## 🔋 BATTERY OPTION (Optional)

### For Power Outages:

**Battery Backup System**:
- 12V 7Ah Lead-acid battery: ~200k
- Battery charger module: ~50k
- Can run ~10 hours on battery (1-2 shelves active)

**Solar Option** (Advanced):
- 20W Solar panel: ~300k
- Solar charge controller: ~100k
- 12V 12Ah battery: ~300k
- Can run indefinitely (but expensive)

**Khuyến nghị**: Không cần battery, dùng nguồn điện thường xuyên

---

## 📝 MAINTENANCE

### Monthly:
- [ ] Check all LED connections
- [ ] Test all buttons
- [ ] Verify WiFi connection
- [ ] Check device status in admin panel

### Quarterly:
- [ ] Clean LED strips
- [ ] Check power supply
- [ ] Update firmware if needed
- [ ] Test full system

### Troubleshooting:

| Issue | Solution |
|-------|----------|
| LEDs not turning on | Check power supply, relay connections |
| WiFi not connecting | Check WiFi credentials, router settings |
| Button not responding | Check button wiring, pull-up resistor |
| Device offline | Check power, WiFi, restart ESP8266 |
| Slow response | Check network latency, server load |

---

## 🚀 FUTURE ENHANCEMENTS

### Phase 2 (Optional):
1. **Status LEDs**: Add RGB LED to show device status
2. **OLED Display**: Show book info on small screen
3. **Sound**: Add buzzer for audio feedback
4. **Multiple Rows**: Support multiple LED strips per shelf
5. **MQTT**: Use MQTT for real-time communication (more efficient)
6. **OTA Updates**: Over-the-air firmware updates
7. **Battery Monitor**: Monitor power consumption
8. **Motion Sensor**: Auto-turn off LEDs if no movement

---

## 📚 LEARNING RESOURCES

### For Team (No IoT Experience):

1. **ESP8266 Basics**:
   - YouTube: "ESP8266 Tutorial for Beginners"
   - Time: 1-2 hours

2. **Arduino Programming**:
   - Arduino.cc documentation
   - Time: 2-3 hours

3. **WiFi Connection**:
   - ESP8266WiFi library examples
   - Time: 1 hour

4. **HTTP Requests**:
   - ESP8266HTTPClient examples
   - Time: 2 hours

**Total Learning Time**: 1 week (part-time)

---

## ✅ IMPLEMENTATION CHECKLIST

### Phase 1: Prototype (1 shelf)
- [ ] Purchase components for 1 shelf
- [ ] Assemble circuit on breadboard
- [ ] Upload firmware
- [ ] Test locally
- [ ] Integrate with backend
- [ ] Test from frontend
- [ ] Fix issues

### Phase 2: Pilot (3 shelves)
- [ ] Purchase components for 3 shelves
- [ ] Assemble permanent circuits
- [ ] Mount in enclosures
- [ ] Install in library
- [ ] Test in production environment
- [ ] Gather user feedback
- [ ] Optimize based on feedback

### Phase 3: Full Deployment (10 shelves)
- [ ] Purchase remaining components
- [ ] Assembly line production
- [ ] Install all units
- [ ] Configure all devices
- [ ] Train staff
- [ ] Monitor system
- [ ] Ongoing maintenance

---

**Status**: Ready for Implementation (Post-Month 3)  
**Estimated Timeline**: 2-3 weeks after main system launch  
**Budget**: ~2,000,000 VNĐ for 10 shelves
