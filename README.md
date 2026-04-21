# 🧬 Guía de Uso: eggNOG-mapper v2.1.13 en el Servidor DEG01

---

## 1. Inicio y Activación

Antes de ejecutar cualquier comando, asegúrate de activar el entorno de Conda configurado para el laboratorio:

```bash
conda activate eggnog-mapper
```

> **Nota:** La variable de entorno `EGGNOG_DATA_DIR` ya está configurada dentro del ambiente para apuntar a la base de datos centralizada en `/db/eggnog_data`.

---

## 2. Comandos de Ejecución Estándar

### A. Modo Diamond (Recomendado — Simulación Web de Alta Precisión)

Este comando combina la velocidad de Diamond con la máxima sensibilidad permitida. Es el ideal para genomas bacterianos completos y metagenomas.

```bash
emapper.py -m diamond \
    -i /ruta/al/archivo/archivo_proteinas.faa \
    -o resultado_anotacion \
    --sensmode more-sensitive \
    --target_orthologs all \
    --go_evidence all \
    --pfam_realign realign \
    --dbmem \
    --cpu 30 \
    --tax_scope Bacteria \
    --temp_dir /data/tmp/
```

### B. Modo HMMER (Máxima Sensibilidad)

Utiliza este modo si necesitas identificar funciones en proteínas muy divergentes o si Diamond no reporta hits suficientes en enzimas clave.

```bash
emapper.py -m hmmer \
    -d 2 \
    -i archivo_proteinas.faa \
    -o resultado_bacteria_hmm \
    --cpu 30
```

---

## 3. Parámetros Críticos Explicados

| Parámetro | Función |
|---|---|
| `--sensmode more-sensitive` | Aumenta la detección de homólogos lejanos (fósiles ortológicos). |
| `--pfam_realign realign` | Obtiene coordenadas exactas de dominios PFAM mediante `phmmer`. |
| `--dbmem` | Carga la base de datos en RAM (~44 GB). Acelera el proceso drásticamente. |
| `--tax_scope Bacteria` | (TaxID 2) Asegura que solo se anoten proteínas del linaje bacteriano. |
| `--temp_dir /data/tmp/` | Vital: redirige los miles de archivos temporales (`emappertmp_*`) fuera de la carpeta del proyecto para mantener el orden. |

---

## 4. Análisis de Resultados

El archivo principal es el `.emapper.annotations`. Las columnas clave para nuestras líneas de investigación son:

- **CAZy:** Identificación de enzimas activas en carbohidratos (crítico para estudios de almidón en el proyecto Caral).
- **COG_cat:** Perfil metabólico general de la comunidad microbiana.
- **KEGG_Pathway:** Reconstrucción de rutas bioquímicas para genómica comparativa.
- **Preferred_name:** Nombre común del gen para anotación de genomas.

---

## 5. Buenas Prácticas y Mantenimiento

1. **Limpieza:** Si olvidas usar `--temp_dir`, recuerda borrar los archivos residuales con:
   ```bash
   rm emappertmp_*
   ```

2. **Permisos:** Si trabajas en directorios compartidos, permite que el resto del equipo vea tus resultados:
   ```bash
   chmod 644 *.emapper.annotations
   ```

---

**Responsable:** Dr. Frank Guzman Escudero  
**Laboratorio:** Biomolecules Laboratory — Facultad de Ciencias de la Salud, UPC  
**Actualizado:** Abril, 2026
