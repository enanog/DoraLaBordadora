# DORA -- Adaptación CNC de Máquina Bordadora

## Descripción General

DORA es un proyecto técnico que convierte una máquina de coser
convencional en una bordadora controlada por CNC.

En lugar de mover la aguja, el sistema desplaza la tela mediante un
pantógrafo de dos ejes (X-Y) accionado por motores paso a paso.

El sistema integra firmware embebido, electrónica de control de motores
y una aplicación de escritorio que procesa los diseños de bordado y se
comunica con la máquina mediante UART.

------------------------------------------------------------------------

## Arquitectura del Sistema

El sistema está dividido en los siguientes bloques:

-   Aplicación de Escritorio (Qt)
-   Comunicación UART (USB)
-   Unidad de Microcontrolador (FRDM-KL25Z)
-   Sistema de Movimiento CNC (2 ejes)
-   Control de Motor AC mediante TRIAC
-   Sensor de Posición de Aguja
-   Interfaz de Display TFT
-   Entrada de Usuario (Teclado 4x4)

------------------------------------------------------------------------

## Componentes de Hardware

### Microcontrolador

-   FRDM-KL25Z
-   Programado en Keil uVision
-   Firmware estructurado en:
    -   Drivers
    -   Dispositivos
    -   Capa de Aplicación
    -   Bucle principal (main)

### Sistema de Movimiento

-   2 motores paso a paso NEMA 17
-   Drivers A4988
-   CNC Shield V4 (interfaz adaptada)
-   Sistema de poleas y correas (pantógrafo X-Y)
-   Finales de carrera para referencia (0,0)

### Control de Aguja

-   Sensor óptico reflectivo CNY70
-   Control de motor AC mediante TRIAC
-   Aislamiento con optoacoplador
-   Control por ángulo de fase para regulación de velocidad

### Display e Interfaz

-   Display TFT 2.2" (320x240, SPI, RGB565)
-   Teclado matricial 4x4
-   Pulsadores para posicionamiento manual en X-Y

### Sistema de Alimentación

-   220V AC para motor principal
-   12V para motores paso a paso
-   5V y 3.3V para lógica
-   Separación de masas entre potencia y control

------------------------------------------------------------------------

## Software

### Aplicación de PC (Qt Creator)

Funciones principales:

-   Cargar imagen .PNG
-   Cargar archivo .CSV procesado
-   Procesar información de píxeles y colores
-   Establecer comunicación UART
-   Enviar instrucciones línea por línea
-   Mostrar simulación y progreso en tiempo real

### Flujo de Procesamiento de Imagen

1.  El usuario procesa la imagen en InkScape.
2.  InkStitch genera el archivo `.csv`.
3.  La aplicación carga:
    -   Imagen original `.png`
    -   Archivo `.csv`
4.  Los datos se serializan y se transmiten al microcontrolador.

------------------------------------------------------------------------

## Comunicación

-   Protocolo: UART (serial asincrónico)
-   Interfaz física: USB
-   Configuración de baudios coincidente entre firmware y aplicación
-   Transmisión línea por línea
-   Comunicación bidireccional para estados y alertas

------------------------------------------------------------------------

## Operación Mecánica

-   El pantógrafo se mueve únicamente cuando la aguja está en posición
    superior.
-   El movimiento de la aguja y los ejes debe estar sincronizado.
-   Rutina de homing al iniciar el sistema.
-   Posicionamiento manual previo al inicio del bordado.

------------------------------------------------------------------------

## Especificaciones Técnicas

-   Área de bordado: 9.5 cm x 9.5 cm
-   Velocidad máxima: 200 puntadas/min
-   Ejes: 2 (X-Y)
-   Número de agujas: 1
-   Formato de diseño: .CSV
-   Comunicación: USB (UART)
-   Dimensiones finales: 81 x 33 x 25 cm
-   Peso aproximado: 25 kg

------------------------------------------------------------------------

## Responsabilidades del Firmware

-   Control de motores paso a paso
-   Rutina de homing
-   Monitoreo de posición de aguja
-   Disparo del TRIAC
-   Gestión de comunicación UART
-   Actualización de display
-   Detección de errores y alertas

------------------------------------------------------------------------

## Limitaciones del Sistema

-   Cambio de hilo manual
-   Montaje manual de tela
-   Corte manual de hilo
-   Operación de una sola aguja
-   Área de bordado limitada

------------------------------------------------------------------------

## Mejoras Futuras

-   Sistema automático de corte de hilo
-   Sensor de detección de hilo cortado
-   Sensor de fin de bobina
-   Mejora en precisión mecánica
-   Soporte para múltiples máquinas en cascada

------------------------------------------------------------------------

## Contexto del Proyecto

Proyecto educativo de sistemas embebidos y mecatrónica desarrollado en
2023.
