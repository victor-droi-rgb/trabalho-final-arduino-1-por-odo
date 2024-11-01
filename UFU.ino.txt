#include <LiquidCrystal_I2C.h>

int LUZ = A1;
int verde = 2;
int vermelho = 3;
int led = 4;
int solo = A0;
int ValorLDR;

LiquidCrystal_I2C lcd(0x20, 16, 2);

void setup() {
  lcd.init();
  lcd.clear();
  lcd.backlight();
  pinMode(verde, OUTPUT);
  pinMode(vermelho, OUTPUT);
  pinMode(led, OUTPUT);
  pinMode(LUZ, INPUT);
  pinMode(solo, INPUT);
  
  Serial.begin(9600);
}

void loop() {
  int leituraLuz = analogRead(LUZ);
  int leituraSolo = analogRead(solo);
  int umidadePercentual = map(leituraSolo, 0, 1023, 100, 0);
  ValorLDR = leituraLuz;

  Serial.print("Valor do LDR: ");
  Serial.println(ValorLDR);

  Serial.print("Umidade do Solo: ");
  Serial.print(umidadePercentual);
  Serial.println("%");

  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Umidade:");
  lcd.setCursor(2, 1);
  lcd.print(umidadePercentual);
  lcd.print("%");

  if (umidadePercentual <= 40){
    digitalWrite(vermelho, HIGH);
    digitalWrite(verde, LOW);
  }
  else{
    digitalWrite(vermelho, LOW);
    digitalWrite(verde, HIGH);
  }
  
  if (ValorLDR > 500) {
    digitalWrite(led, HIGH);
  } else { 
    digitalWrite(led, LOW);
  }
  
  delay(1000);
}
