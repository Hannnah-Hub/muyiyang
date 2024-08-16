## 识别到50cm内有物体，改变灯光颜色
```C++
#include <Adafruit_NeoPixel.h>
#define PIN 6         // LED灯带连接的数字引脚
#define NUM_LEDS 30   // 灯带上LED的数量

Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUM_LEDS, PIN, NEO_GRB + NEO_KHZ800);

#define trigPin 9    // 超声波传感器的Trig引脚
#define echoPin 8    // 超声波传感器的Echo引脚

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;

  if (distance < 50) { // 如果检测到物体距离小于50cm
    // 启动跑马灯效果
    for(int i=0; i<strip.numPixels(); i++) {
      strip.setPixelColor(i, strip.Color(255, 255, 255)); // 白色
      strip.show();
      delay(50);
      strip.setPixelColor(i, strip.Color(255, 255, 0)); // 黄色
      strip.show();
      delay(50);
    }
  } else {
    // 常亮绿色
    for(int i=0; i<strip.numPixels(); i++) {
      strip.setPixelColor(i, strip.Color(0, 255, 0)); // 绿色
    }
    strip.show();
  }
  delay(500); // 稳定延时
}
```

## 缓慢改变灯光颜色
```C++
#include <Adafruit_NeoPixel.h>
#define PIN 6         // LED灯带连接的数字引脚
#define NUM_LEDS 30   // 灯带上LED的数量

Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUM_LEDS, PIN, NEO_GRB + NEO_KHZ800);

#define trigPin 9    // 超声波传感器的Trig引脚
#define echoPin 8    // 超声波传感器的Echo引脚

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
  
  // 初始化为绿色常亮
  for(int i = 0; i < strip.numPixels(); i++) {
    strip.setPixelColor(i, strip.Color(0, 255, 0)); // 绿色
  }
  strip.show();
}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;

  if (distance < 50) { // 如果检测到物体距离小于50cm
    // 缓慢变成白色
    for(int j = 0; j <= 255; j += 5) {
      for(int i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i, strip.Color(j, 255, j)); // 从绿色(0,255,0)渐变到白色(255,255,255)
      }
      strip.show();
      delay(30); // 调整渐变速度
    }
  } else {
    // 缓慢恢复为绿色
    for(int j = 255; j >= 0; j -= 5) {
      for(int i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i, strip.Color(0, 255, j)); // 从白色(255,255,255)渐变到绿色(0,255,0)
      }
      strip.show();
      delay(30); // 调整渐变速度
    }
  }
  delay(500); // 稳定延时
}
```
