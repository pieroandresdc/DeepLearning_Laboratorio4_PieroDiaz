# Laboratorio 4: ComfyUI Identity Synthesis and Transformation Pipeline

**Curso:** Deep Learning  
**Alumno:** Piero Andrés Díaz Carpio  
**Integrantes del Equipo Analizado:** Piero, Felipe y Luis

## 1. Descripción del Proyecto
El objetivo de este laboratorio fue diseñar un *workflow* en ComfyUI para aplicar transformaciones visuales a fotografías reales de los integrantes del equipo en distintos contextos y estilos, estudiando el límite exacto entre la alteración artística y la preservación de la identidad. Se optó por una arquitectura **Image-to-Image** utilizando el modelo base Stable Diffusion 1.5 en el entorno de ComfyUI Cloud para optimizar los tiempos de inferencia y cumplir con la restricción de usar nuestros propios rostros.

---

## 2. Metodología y Configuración (Workflow)
El pipeline principal consta de los siguientes nodos clave:
*   **Load Image:** Entrada del dataset real.
*   **VAE Encode & Decode:** Transición entre píxeles y espacio latente.
*   **KSampler:** Cerebro generador. El parámetro crítico analizado en este laboratorio fue el **`denoise`** (fuerza de eliminación de ruido), el cual dictó matemáticamente el límite de preservación de identidad.
*   **Archivo adjunto:** El workflow completo se encuentra en este repositorio en el archivo `DL_Lab4_Image-to-Image.json`.

---

## 3. Resultados Individuales

### A. Cambios Leves (Denoise: ~0.30)
**Objetivo:** Modificaciones de iluminación y contexto manteniendo la identidad intacta (Ej. *Golden hour, retrato de estudio*).
*   **Análisis:** Al configurar el `denoise` en un valor bajo, el modelo alteró solo el 30% de la imagen. La estructura facial, proporcionesyrecognoscibilidad mío, de Felipe y de Luis se preservaron en lineas generales. El fondo no pmucho, pero la iluminación se adaptó correctamente  al prompt.

### B. Cambios Moderados (Denoise: ~0.65 - 0.70)
**Objetivo:** Ambientes marcados (Ej. *Estética Cyberpunk, laboratorio futurista*) manteniendo una recognoscibilidad razonable.
*   **Análisis:** Para lograr que la IA genere la ciudad cyberpunk, tuvimos que subir el `denoise` y usar *Prompt Weighting* (ej. `(neon lights:1.5)`). En este nivel, la identidad comienza a suavizarse. Las facciones se mantienen reconocibles, pero el modelo empieza a fusionar nuestros rasgos con el estilo artístico del modelo generativo, lamentablemente en estos casos la IA comenzó a alucinar y dar resultados sin ningun sentido.

### C. Cambios Fuertes (Denoise: ~0.85 - 0.95)
**Objetivo:** Transformaciones semánticas agresivas (Ej. *Astronauta, científico 1920*) para encontrar el límite de la identidad .
*   **Análisis:** Al permitir que el modelo sobrescriba hasta el 90% de la imagen, la transformación del vestuario y el entorno es más observable, más apreciable. Sin embargo, documentamos que **este es el punto de quiebre donde la identidad se degrada totalmente**. Aunque se respeta la pose original, los rostros generados alucinan facciones nuevas que ya no nos pertenecen pero que sí le permiten al modelo trabajar con acercarse y cumplir con el prompt.

---

## 4. Resultados Grupales Combinados [2, 7]
**Escenario:** Equipo de investigación en Inteligencia Artificial en un laboratorio futurista.
Se utilizó una fotografía real donde aparecen los integrantes juntos para asegurar la coherencia en la escala y composición original.

*   **Análisis de Dificultad:** Modificar una imagen con múltiples integrantes presentó la mayor limitación técnica del pipeline. Si se usaba un `denoise` agresivo, el modelo sufría para mantener la atención dividida y comenzaba a mezclar las facciones entre los integrantes. Para solucionarlo y mantener las identidades separadas, fue obligatorio usar un `denoise` conservador (~0.45); sin embargo, esto a costa de que pudisra cumplir con lo especificado en el prompt en cuanto a contexto, fondo, vestimenta, ect.

---

## 5. Conclusiones y Limitaciones Observadas
1.  **Preservación vs. Transformación:** Existe una relación matemáticamente inversamente proporcional entre la fuerza del cambio de estilo y la fidelidad biométrica del sujeto.
2.  **Ignorancia del Prompt en Image-to-Image:** Se descubrió que en la arquitectura Image-to-Image, un `denoise` bajo (para cuidar la identidad) causa que el modelo ignore las instrucciones de fondo o contexto del prompt, obligando al ingeniero a buscar un balance empírico meticuloso.
3.  **Gestión de Recursos:** Optar por ComfyUI Cloud mitigó las limitaciones de hardware (VRAM) experimentadas en implementaciones locales de Colab previas.
