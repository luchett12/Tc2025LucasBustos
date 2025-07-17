#

# Informe Técnico: Compilador para un Subconjunto de C++

**Título del Trabajo:**  
Diseño e Implementación de un Compilador para un Subconjunto de C++

**Integrante:**  
Bustos Lucas

**Materia:**  
Técnicas de Compilación

**Profesor:**  
Ameri, fransisco
**Fecha:**  
17/07/2025

---

## 1. Introducción

Este proyecto aborda el diseño e implementación de un compilador completo para un subconjunto del lenguaje C++. El objetivo es aplicar los conceptos teóricos y prácticos de la materia, desarrollando una herramienta capaz de realizar el proceso de compilación completo: análisis léxico, sintáctico, semántico, generación de código intermedio y optimización.

El compilador utiliza ANTLR4 para la generación de los analizadores léxico y sintáctico. Se ha puesto especial énfasis en el reporte detallado de errores, la generación de código de tres direcciones y la optimización básica del código intermedio.

---

## 2. Análisis del Problema

El desafío principal es procesar código fuente en un subconjunto de C++ y traducirlo a una representación intermedia optimizada. El compilador debe:

- **Analizar y Validar:** Reconocer la estructura léxica y sintáctica, y verificar la coherencia semántica.
- **Soportar un Subconjunto Específico:** Manejar los tipos de datos, estructuras de control y elementos definidos en la consigna.
- **Detectar Errores:** Reportar errores léxicos, sintácticos y semánticos (variables duplicadas, uso de variables no declaradas, etc.).
- **Generar Advertencias:** Notificar sobre símbolos declarados pero no utilizados.
- **Generar Código Intermedio:** Producir código de tres direcciones legible y optimizado.

---

## 3. Diseño de la Solución

### 3.1. Arquitectura General

El compilador sigue un flujo modular clásico:

- **Entrada:** Lectura de un archivo fuente (`input/ejemplo_correcto.c`).
- **Front-End:**
  - **Análisis Léxico:** Lexer de ANTLR4 convierte caracteres en tokens.
  - **Análisis Sintáctico:** Parser de ANTLR4 valida la estructura y construye el árbol sintáctico.
  - **Análisis Semántico:** Un listener personalizado (`CustomListener`) construye la tabla de símbolos y valida semántica.
- **Back-End:**
  - **Generación de Código Intermedio:** Un visitor (`CustomVisitor`) recorre el árbol y genera código de tres direcciones.
  - **Optimización:** Se eliminan instrucciones duplicadas y se reportan estadísticas.
- **Salida:** Se muestran en consola los resultados, errores, advertencias y se generan archivos de código intermedio y optimizado.

### 3.2. Fases de Compilación

- **Análisis Léxico:**  
  Gramática `compiladores.g4` define tokens, palabras reservadas, identificadores, literales y operadores.  
  Se reportan errores léxicos en consola.

- **Análisis Sintáctico:**  
  El parser valida la secuencia de tokens y construye el árbol sintáctico.  
  Se reportan errores sintácticos y se muestra el árbol en consola y en una ventana gráfica con zoom y scroll.

- **Análisis Semántico:**  
  El listener gestiona la tabla de símbolos, verifica tipos, detecta errores de declaración y uso, y genera advertencias.

- **Generación de Código Intermedio:**  
  El visitor traduce el árbol a código de tres direcciones, manejando expresiones, estructuras de control y llamadas a funciones.

- **Optimización:**  
  Se eliminan instrucciones duplicadas y se reportan estadísticas de optimización.

---

## 4. Implementación

### 4.1. Gramática ANTLR4

La gramática `compiladores.g4` define la estructura del lenguaje, incluyendo:

- **Regla Inicial:** `programa: instrucciones EOF;`
- **Declaraciones:** Variables, arrays y funciones.
- **Expresiones:** Precedencia de operadores, expresiones aritméticas y lógicas.
- **Sentencias:** If, while, for, return, llamadas a función, etc.

### 4.2. Tabla de Símbolos

La clase `TablaSimbolos` gestiona una lista de scopes (ámbitos), cada uno con sus símbolos.  
Cada símbolo contiene: nombre, tipo, categoría (variable, función, parámetro), ámbito, línea, columna y detalles.

- **Métodos clave:**
  - `addSymbol`: Inserta un símbolo en el scope actual.
  - `getSymbol`: Busca un símbolo en los scopes.
  - `getTablaFormateada`: Imprime la tabla alineada.

### 4.3. Algoritmos de Cada Fase

- **Análisis Semántico (`CustomListener`):**
  - Detecta declaraciones duplicadas, uso de variables no declaradas, etc.
  - Gestiona ámbitos para variables globales y locales.
  - Marca símbolos como usados/no usados.

- **Generación de Código (`CustomVisitor`):**
  - Genera instrucciones de tres direcciones para asignaciones, operaciones, saltos, llamadas, retornos.
  - Usa variables temporales para subexpresiones.
  - Aplica optimización simple (eliminación de duplicados).

- **Visualización del AST:**
  - Imprime el árbol en consola (formato textual).
  - Muestra el árbol en una ventana Swing con scroll y zoom, y controles de acercar/alejar.

---

## 5. Ejemplos y Pruebas

- **Errores detectados:**  
  - Uso de variables no declaradas.
  - Uso de variables no inicializadas.
  - Declaración duplicada de variables.

- **Advertencias:**  
  - (Opcional) Variables declaradas pero no usadas.

- **Código intermedio generado:**  
  Ejemplo de salida:
  ```
  0: // Código de tres direcciones generado
  DECLARE resultado int
  t1 = a+b
  resultado = t1
  ...
  ```

- **Optimización:**  
  - Instrucciones duplicadas eliminadas.
  - Estadísticas de reducción impresas en consola.

---

## 6. Dificultades Encontradas y Soluciones

- **Ambigüedad en la gramática:**  
  Se resolvió ajustando reglas y precedencia en la gramática ANTLR.

- **Errores de null en el visitor:**  
  Se agregaron chequeos de nulidad antes de procesar nodos opcionales.

- **Gestión de ámbitos:**  
  Se ajustó la lógica para que los símbolos de todos los scopes se mantengan hasta el final del análisis.

- **Visualización del AST:**  
  Se mejoró la ventana gráfica con scroll y zoom, y se imprime el árbol en consola para facilitar el análisis.

---

## 7. Conclusiones

El proyecto permitió aplicar de manera práctica el flujo completo de un compilador.  
ANTLR4 resultó ser una base robusta para el front-end, y la implementación manual de la tabla de símbolos, el generador de código y el optimizador consolidó la comprensión de los procesos del back-end.

El compilador cumple con los requisitos funcionales:  
- Análisis semántico detallado  
- Generación de código de tres direcciones  
- Optimización básica  
- Reporte claro y modular

El diseño modular facilita su extensión futura para soportar más características del lenguaje C++ o implementar optimizaciones más avanzadas.

---

## 8. Manual de Usuario

**Requisitos Previos:**
- Java JDK 8+ (JAVA_HOME configurado)
- Apache Maven 3.x
- Editor recomendado: VS Code o IntelliJ IDEA

**Compilar y ejecutar:**
```sh
cd /Users/lucasbustos/Desktop/Repo/TC2024/Parcial_2-TC
mvn clean install
mvn compile exec:java -Dexec.mainClass=compiladores.App -Djava.awt.headless=false
```

**Personalizar entrada:**
- Edita el archivo `input/ejemplo_correcto.c` con tu código.
- Vuelve a ejecutar el comando anterior para ver el reporte actualizado.

**Archivos generados:**
- `input/ejemplo_correcto_codigo_intermedio.txt`
- `input/ejemplo_correcto_codigo_optimizado.txt`

**Visualización del AST:**
- El árbol sintáctico se muestra en consola y en una ventana gráfica con zoom y scroll.

---****
