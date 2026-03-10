const int ENA = 5;
const int IN1 = 6;
const int IN2 = 7;
const int ENB = 9;
const int IN3 = 8;
const int IN4 = 10;

const int sensorPins[8] = {A0, A1, A2, A3, A4, A5, 2, 3};
bool s[8];

const float weights[8] = {-15, -6, -2, -1, 1, 2, 6, 15};

float Kp = 1.0;
float Kd = 10.0;

int baseSpeed = 90;
float lastError = 0.0;

void setup() {
  Serial.begin(9600);   // 🔥 Debugging enabled

  pinMode(ENA, OUTPUT); pinMode(IN1, OUTPUT); pinMode(IN2, OUTPUT);
  pinMode(ENB, OUTPUT); pinMode(IN3, OUTPUT); pinMode(IN4, OUTPUT);

  for (int i = 0; i < 8; i++) pinMode(sensorPins[i], INPUT);

  delay(3000);
}

void loop() {
  readSensors();

  // 🔥 First check special cases
  if (handleSpecialCases()) return;

  float error = calculateWeightedError();

  if (error == 100.0) {
    recoverLine(lastError);
    return;
  }

  if (abs(error) > 8) {
    if (error > 0)
      setMotors(120, -120);
    else
      setMotors(-120, 120);
    lastError = error;
    return;
  }

  float adjustment = (error * Kp) + ((error - lastError) * Kd);
  lastError = error;

  setMotors(baseSpeed + adjustment, baseSpeed - adjustment);
}

void readSensors() {
  Serial.print("Sensors: ");

  for (int i = 0; i < 8; i++) {
    s[i] = digitalRead(sensorPins[i]);
    Serial.print(s[i]);
    Serial.print(" ");
  }

  Serial.println();
}

float calculateWeightedError() {
  float sum = 0;
  int count = 0;

  for (int i = 0; i < 8; i++) {
    if (s[i]) {
      sum += weights[i];
      count++;
    }
  }

  if (count == 0) return 100.0;

  return sum / count;
}

/* ---------- SPECIAL CASE LOGIC ---------- */
bool handleSpecialCases() {

  // 🔥 2 SECOND FORCED TURN CASES
  if (
      (!s[0] && !s[1] && !s[2] &&  s[3] &&  s[4] && !s[5] &&  s[6] &&  s[7]) ||   // 00011011
      (!s[0] && !s[1] &&  s[2] &&  s[3] && !s[4] && !s[5] &&  s[6] &&  s[7])      // 00110011
     )
  {
    Serial.println("Case: 2 SECOND RIGHT TURN");

    unsigned long startTime = millis();

    while (millis() - startTime < 500) {   // 0.5 seconds
      setMotors(130, 60);  // Controlled right turn
    }

    return true;   // Skip PID after this
  }

  return false;
}
/* ----------------------------------------- */

void recoverLine(float memory) {
  Serial.println("Recovering Line");

  int dir = (memory < 0) ? -1 : 1;

  setMotors(110 * dir, -110 * dir);

  while (true) {
    for (int i = 0; i < 8; i++) {
      if (digitalRead(sensorPins[i])) return;
    }
  }
}

void setMotors(int left, int right) {
  left = constrain(left, -255, 255);
  right = constrain(right, -255, 255);

  digitalWrite(IN1, left > 0);
  digitalWrite(IN2, left < 0);
  analogWrite(ENA, abs(left));

  digitalWrite(IN3, right > 0);
  digitalWrite(IN4, right < 0);
  analogWrite(ENB, abs(right));
}
