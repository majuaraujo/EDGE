## HEATHFLOW-PRO - Termômetro digital

## Descrição do problema

O monitoramento dos indicadores de saúde pode ser um desafio significativo para os pacientes, pois envolve uma série de fatores que podem criar preocupações e dificuldades em sua jornada de autocuidado.
Isso se torna possível através de um sistema de monitoramento dos indicadores de saúde. Este termômetro é uma ideia inicial para que o usuário tenha ciência de como está sua temperatura no dia e se ele deve se preocupar ou não.


## Funcionalidades

O Arduino UNO utiliza a biblioteca para mostrar no display a temperatura, os leds amarelos são acionados quando a temperatura atinge 37/38 e o buzzer é acionado quando a tempuratura ultrapassa 38, acionando o led vermelho.

## Tecnologias e componentes utilizados

- Linguagem C
- Simulador Arduino Tinkercad
- Protoboard
- Placa de Arduino
- LEDs (Verde, Amarelo, Vermelho e Azul)
- Resistores
- Buzzer
- Jumpers
- Display
- Sensor de temperatura
- Liquid Crystal Library

## Rodando o projeto

Para rodar basta instalar, você pode acessar a simulação que fizemos [neste link](https://www.tinkercad.com/things/1jidRGBnIJg-termometro-digital-gs?sharecode=zQh6-Dvs2_riTTLnnm0ICcUSF7HVRCk4eXIiR7YdQCg) ou então montar o circuito da imagem e rodar o código a seguir:

![MicrosoftTeams-image](https://github.com/felpschneider/vinheria-agnello/assets/89404927/8b1ffbeb-e785-4b10-92ac-4e6b7521da7e)

# Links
Abaixo você pode acessar os links da apresentação do circuito
- [Video explicativo](https://youtu.be/vtZ2tf5-020)
- [Link da simulação no Tinkercad](https://www.tinkercad.com/things/1jidRGBnIJg-termometro-digital-gs?sharecode=zQh6-Dvs2_riTTLnnm0ICcUSF7HVRCk4eXIiR7YdQCg)

# Elaborador
- Maria Julia Araujo Rodrigues - RM 553384  1ESPR


## Código Fonte

#include <LiquidCrystal.h>

LiquidCrystal LCD(13,12,11,10,9,8);

int SensorTemperatura=0;

int AlertaHipotermia=1;

int AlertaNormal=2;

int AlertaFebre=3;

int AlertaFebreAlta=4;

int AlertaHipertermia=5;

int Buzzer=6;

int treme=7;

void setup() 
{
  	pinMode(AlertaHipotermia, OUTPUT);
	pinMode(AlertaNormal, OUTPUT);
  	pinMode(AlertaFebre, OUTPUT);
  	pinMode(AlertaFebreAlta, OUTPUT);
  	pinMode(AlertaHipertermia, OUTPUT);
	LCD.begin(16,2);
  	pinMode(Buzzer, OUTPUT);
  	pinMode(treme, OUTPUT);
  	LCD.print("Ola Professor!!!");
  	delay(3000);
  	LCD.clear();
  	LCD.print("Iniciando...");
  	delay(1500);
}


void loop() 
{
	int SensorTensao=analogRead(SensorTemperatura);
	float Tensao=SensorTensao;
	Tensao/=1024;
	float TemperaturaC=(Tensao+0.15)*100;
  	LCD.clear();
    LCD.print("Temperatura:");
  	LCD.setCursor(0,1);
	LCD.print(TemperaturaC);
  	LCD.write(B10110010);
  	LCD.print("C");
  	
	
  if(TemperaturaC<=35)
  	{
  		digitalWrite(AlertaHipotermia, HIGH);
  		digitalWrite(AlertaNormal, LOW);
    	digitalWrite(AlertaFebre, LOW);
    	digitalWrite(AlertaFebreAlta, LOW);
    	digitalWrite(AlertaHipertermia, LOW);
      	digitalWrite(Buzzer, 0);
    	analogWrite(treme, 0);

    }
  else if(TemperaturaC>=35 && TemperaturaC<=37.5)
  {
  		digitalWrite(AlertaHipotermia, LOW);
  		digitalWrite(AlertaNormal, HIGH);
    	digitalWrite(AlertaFebre, LOW);
    	digitalWrite(AlertaFebreAlta, LOW);
    	digitalWrite(AlertaHipertermia, LOW);
      	digitalWrite(Buzzer, 0);
    	analogWrite(treme, 0);

  	}
  else if(TemperaturaC>=37.5 && TemperaturaC<=39.5)
  {
    	digitalWrite(AlertaHipotermia, LOW);
  		digitalWrite(AlertaNormal, LOW);
    	digitalWrite(AlertaFebre, HIGH);
    	digitalWrite(AlertaFebreAlta, LOW);
    	digitalWrite(AlertaHipertermia, LOW);
    	tone(Buzzer, 50, 400);
      	digitalWrite(Buzzer, 1);
    	analogWrite(treme, 150);

    }
  else if(TemperaturaC>=39.5 && TemperaturaC<=41)
  {
    	digitalWrite(AlertaHipotermia, LOW);
  		digitalWrite(AlertaNormal, LOW);
    	digitalWrite(AlertaFebre, LOW);
    	digitalWrite(AlertaFebreAlta, HIGH);
    	digitalWrite(AlertaHipertermia, LOW);
    	tone(Buzzer, 100, 400);
      	digitalWrite(Buzzer, 1);
    	analogWrite(treme, 200);

    }
  else if(TemperaturaC>=41)
  {
    	digitalWrite(AlertaHipotermia, LOW);
  		digitalWrite(AlertaNormal, LOW);
    	digitalWrite(AlertaFebre, LOW);
    	digitalWrite(AlertaFebreAlta, LOW);
    	digitalWrite(AlertaHipertermia, HIGH);
    	tone(Buzzer, 150, 400);
      	digitalWrite(Buzzer, 1);
    	analogWrite(treme, 255);
    }
  else
  {
    digitalWrite(AlertaHipotermia, LOW);
  		digitalWrite(AlertaNormal, LOW);
    	digitalWrite(AlertaFebre, LOW);
    	digitalWrite(AlertaFebreAlta, LOW);
    	digitalWrite(AlertaHipertermia, LOW);
      	digitalWrite(Buzzer, 0);
    	analogWrite(treme, 0);
  }
  	delay(100);
}
