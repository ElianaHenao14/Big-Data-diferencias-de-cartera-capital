# 3. Arquitectura Propuesta 

## Arquitectura Medallion

### Bronze
Datos sin transformar.

**Archivos fuente:**

**Plano Día Antes**
| NIT | Capital |

**Desembolsos**
| NIT | Valor Desembolso |

**Recaudos**
| NIT | Efectivo | Cheque |

**Plano Día Diferencia**
| NIT | Capital |

---

### Silver
Procesos de transformación.
Basados en el flujo KNIME:

**Limpieza**
- Eliminación de registros inválidos.
- Validación de tipos de datos.
- Conversión de NIT a texto.

**Enriquecimiento**
Agrupación:
- Capital Día Antes por NIT
- Desembolsos por NIT
- Pagos por NIT

**Reglas de Negocio**
Capital Esperado:

 Capital Día AntesDesembolsosPagos

### Gold
Información lista para análisis.

**Variables finales:**
| NIT | Capital Esperado | Capital Real | Diferencia |

**Clasificación:**
Sin diferencia
Diferencia positiva
Diferencia negativa
