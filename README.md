# Dashboard Financiero — SLEP Petorca

Dashboard financiero del **Servicio Local de Educación Pública de Petorca** (código institucional 0949).

🌐 **Versión publicada:** https://finanzas-slep-petorca.github.io/dashboard-financiero/

## Qué contiene

Reporte interactivo HTML autogenerado con:

- 📊 Flujo de caja diario proyectado por Fuente de Financiamiento (FF) hasta fin de año
- 🍎 Flujo mensual JARDINES (JUNJI) — Ingresos por concepto + Gastos por subtítulo + Saldo + Flujo acumulado
- 🎓 Flujo mensual EDUCACIÓN (Escuelas) — Consolidado y por cada FF individual (Subv. General, SEP, PIE, Mantenimiento, Pro-Retención, Aporte Fiscal, etc.)
- 📋 Salud financiera por FF con semáforos de déficit/superávit
- 💰 Cruce ACEPTA ↔ SIGFE (Chile Paga) con estado contable autoritativo
- 📈 Cartera y caja proyectada por FF
- 🏢 Top proveedores y deuda flotante real

### Funcionalidades del dashboard

- **Celdas editables** en los flujos mensuales para simular escenarios
- **Recálculo automático** de totales, subtotales, saldos y flujo de caja
- **Sincronización** consolidado ↔ vista por FF
- **Filtros** por programa (P01/P02), FF, fecha
- **Exportar a Excel** con fórmulas reales
- **Importar Excel** previamente editado
- **Imprimir / Guardar PDF** en formato A3 landscape
- **Cortar al mes**: limitar reporte a un mes específico (ej. solo Q1)

## Cómo actualizar la publicación

El dashboard se regenera localmente desde el código en
[`Finanzas-SLEP-Petorca/slep-petorca-finance`](https://github.com/Finanzas-SLEP-Petorca/slep-petorca-finance)
(repo privado con código y datos).

Para actualizar la versión publicada aquí:

```bash
# 1) Regenerar el dashboard en local
cd slep-petorca-finance
python -m etl.pipeline --carga CARGA --fecha-corte 2026-05-31
python -m reports.dashboard

# 2) Copiar el HTML generado al repo de publicación
cp reports/output/dashboard.html ../dashboard-financiero/index.html

# 3) Commit + push (el workflow GitHub Actions publica automáticamente)
cd ../dashboard-financiero
git add index.html
git commit -m "Actualizar dashboard - 2026-05-31"
git push
```

GitHub Actions ejecuta `.github/workflows/deploy.yml` y publica en https://finanzas-slep-petorca.github.io/dashboard-financiero/ en ~1 minuto.

## Arquitectura del proyecto completo

```
slep-petorca-finance/        (repo PRIVADO con código + datos)
├── CARGA/                   (archivos SIGFE, ACEPTA, banco, remuneraciones)
├── data/slep.db             (base SQLite — NO se sube)
├── etl/                     (loaders y pipeline)
├── analytics/               (motores de cruce, flujo de caja, asignación FF)
└── reports/
    └── output/
        └── dashboard.html   ← se copia a este repo público

dashboard-financiero/        (este repo PÚBLICO con solo HTML)
├── index.html
├── README.md
└── .github/workflows/deploy.yml
```

## Fuentes de datos

- **SIGFE 2.0**: Balance, Estado Ejecución Presupuestaria, Mayor Contable, Listado Pagos, Chile Paga, Cartera Devengo, etc.
- **ACEPTA / SGDTE**: Reporte de facturas en pipeline operativo
- **BancoEstado**: Cartolas + saldos diarios por cuenta corriente
- **CasChile**: Maestro mensual de remuneraciones por centralización
- **MINEDUC**: Subvenciones recibidas, calendario oficial

## Contexto institucional

SLEP Petorca opera con 2 programas presupuestarios:
- **P01** Gastos Administrativos (Aporte Fiscal Libre, ~$4.450M Ley Vigente 2026)
- **P02** Servicio Educativo (Subvenciones MINEDUC + JUNJI, ~$42.890M Ley Vigente 2026)

Y 17 cuentas BancoEstado, cada una asociada a una Fuente de Financiamiento específica con uso restringido según normativa MINEDUC.

---

*Generado automáticamente. Última actualización: ver fecha del commit.*
