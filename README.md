# p7_estrellas
import controlP5.*;

ControlP5 cp5;
ArrayList<Star> estrellas = new ArrayList<Star>();

float tamañoEstrella = 8;  
float colorEstrella = 0;  
int numPuntas = 5;  
float colorFondo = 0;  // Nuevo controlador para el fondo

void setup() {
  size(600, 600);
  background(0);
  colorMode(HSB, 360, 100, 100);

  cp5 = new ControlP5(this);
  
  // Slider para el tamaño de las estrellas
  cp5.addSlider("tamañoEstrella")
    .setPosition(10, 10)
    .setSize(150, 20)
    .setRange(3, 30)  
    .setValue(8);
  
  // Slider para el color de las estrellas
  cp5.addSlider("colorEstrella")
    .setPosition(10, 40)
    .setSize(150, 20)
    .setRange(0, 360)  
    .setValue(0);  

  // Slider para cambiar la cantidad de puntas de la estrella
  cp5.addSlider("numPuntas")
    .setPosition(10, 70)
    .setSize(150, 20)
    .setRange(3, 12)  // De 3 a 12 puntas
    .setValue(5);
  
  // Slider para cambiar el color del fondo
  cp5.addSlider("colorFondo")
    .setPosition(10, 100)
    .setSize(150, 20)
    .setRange(0, 360)  
    .setValue(0);
  
  // Botón para guardar PNG
  cp5.addBang("guardarPNG")
    .setPosition(10, 130)
    .setSize(100, 30)
    .setLabel("Guardar PNG")
    .onPress(new CallbackListener() {
      public void controlEvent(CallbackEvent theEvent) {
        guardarPNG();
      }
    });
}

void draw() {
  if (colorFondo == 0) {
    background(0); // Asegura que uno de los colores sea negro
  } else {
    background(colorFondo, 100, 20);
  }
  
  // Dibujar estrellas
  for (Star e : estrellas) {
    drawStar(e.pos.x, e.pos.y, e.tamaño, numPuntas, colorEstrella);
  }
  
  // Dibujar conexiones entre estrellas en orden de creación
  stroke(colorEstrella, 50, 100);
  for (int i = 0; i < estrellas.size() - 1; i++) {
    line(estrellas.get(i).pos.x, estrellas.get(i).pos.y, estrellas.get(i + 1).pos.x, estrellas.get(i + 1).pos.y);
  }
}

void mousePressed() {
  if (mouseX > 180) { // Evita clics en el área de controles
    estrellas.add(new Star(new PVector(mouseX, mouseY), tamañoEstrella));
  }
}

void guardarPNG() {
  save("constelacion-" + frameCount + ".png");
}

// Función para dibujar una estrella con puntas
void drawStar(float x, float y, float radio, int puntas, float hueColor) {
  float angulo = TWO_PI / puntas;
  float mitadAngulo = angulo / 2.5;  
  float radioInterno = radio / 2;  

  fill(hueColor, 80, 100);
  noStroke();
  beginShape();
  for (float a = 0; a < TWO_PI; a += angulo) {
    float sx = x + cos(a) * radio;
    float sy = y + sin(a) * radio;
    vertex(sx, sy);
    sx = x + cos(a + mitadAngulo) * radioInterno;
    sy = y + sin(a + mitadAngulo) * radioInterno;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}

class Star {
  PVector pos;
  float tamaño;
  
  Star(PVector pos, float tamaño) {
    this.pos = pos;
    this.tamaño = tamaño;
  }
}
