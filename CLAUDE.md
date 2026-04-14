# CLAUDE.md

## Contexto del negocio
Este proyecto pertenece a OHS Training GT, una empresa enfocada en salud y seguridad ocupacional en Guatemala.

## Objetivo de Claude
Claude debe ayudar a crear, mejorar y organizar:
- propuestas comerciales
- cotizaciones
- catálogos de productos y servicios
- fichas técnicas
- programas de capacitación
- contenido web
- textos publicitarios y corporativos
- documentos de cumplimiento en salud y seguridad ocupacional
- Contenido para cursos referentes a salud y seguridad ocupacional 
- contabilidad del negocio
- apoyo a operaciones de logistica, compra, venta, distribución y elaboración

## Perfil de la empresa
OHS Training GT ofrece:
- capacitaciones en salud y seguridad ocupacional
- primeros auxilios
- RCP
- evacuación y respuesta a emergencias
- botiquines empresariales
- señalización
- extintores
- insumos médicos y equipo de seguridad

## Reglas de redacción
- Escribir siempre en español.
- Usar un tono profesional, claro y comercial.
- Evitar lenguaje demasiado técnico si el documento es para clientes.
- Si el documento es legal o técnico, usar un tono más formal y preciso.
- Redactar con enfoque en empresas de Guatemala.
- Priorizar claridad, cumplimiento y presentación profesional.

## Reglas de formato
- Moneda oficial: quetzales (Q).
- Incluir estructura ordenada con títulos y subtítulos.
- Cuando se trate de propuestas, incluir:
  - objetivo
  - alcance
  - metodología
  - duración
  - entregables
  - precio
  - vigencia
- Cuando se trate de catálogos, incluir:
  - nombre del producto o servicio
  - descripción
  - beneficios
  - presentación
  - cumplimiento normativo si aplica

## Identidad visual
- Marca: OHS Training GT
- Estilo visual: corporativo, limpio y profesional
- Priorizar documentos sobrios, atractivos y fáciles de leer
- Usar tipografía Century Gothic cuando se solicite
- Evitar diseños recargados

## Cumplimiento normativo
Claude debe considerar normativa y contexto de Guatemala cuando aplique.
Si el usuario pide base legal, responder con enfoque en legislación guatemalteca de salud y seguridad ocupacional.

## Reglas de trabajo
- Si falta información crítica para una cotización, Claude debe indicarlo claramente.
- Si el usuario pide precios, presentarlos en quetzales.
- Si el usuario pide una propuesta, asumir estructura ejecutiva lista para cliente.
- Si el usuario pide algo “tipo catálogo”, priorizar diseño comercial y lenguaje persuasivo.
- Si el usuario pide versión web, estructurar el contenido para sitio corporativo.
- Si el usuario pide algo editable, preparar contenido fácil de pasar a Word, Canva o HTML.

## Cosas que Claude debe recordar
- OHS Training GT trabaja para empresas.
- El enfoque principal es prevención, cumplimiento, capacitación y respuesta a emergencias.
- El material debe verse profesional y vendible.
- No usar lenguaje infantil ni informal, salvo que el usuario lo pida.

## Datos Adicionales 
- dominio: gtohstraining.com
- correo: info@ohstraining.com
- telefono: (502) 56223829

---

## ERP — Estado actual del proyecto

### Archivo principal
`OHS_ERP.html` — aplicación React 18 de un solo archivo, compilada con Babel Standalone desde CDN. Sin dependencias de build. Datos persistidos en `localStorage` con prefijo `ohs_`.

### GitHub
- Repositorio: `https://github.com/pmeji/ohs-erp` (actualizar URL si cambia)
- GitHub Pages: `https://pmeji.github.io/ohs-erp/OHS_ERP.html`
- Token de acceso: ver configuración local (no subir al repositorio)
- Para hacer push de cambios usar: `git remote set-url origin https://TOKEN@github.com/USUARIO/ohs-erp.git`

### Usuarios del sistema
| Usuario | Clave | Rol |
|---------|-------|-----|
| pmejia | 123456 | admin |
| greynoso | 123456 | admin |

Ambos tienen acceso completo. El rol se controla con `canEdit = currentUser?.role === 'admin'` y se provee via `AuthContext`.

### Módulos implementados
| ID | Nombre | Notas |
|----|--------|-------|
| dashboard | Dashboard | Resumen general |
| reportes | Reportes | Análisis ventas/compras/inventario |
| presupuestos | Presupuestos | Planificación por período, comparativo vs real |
| proyectos | Proyectos | Tareas, fechas, estados, vinculación con cotizaciones |
| cotizaciones | Cotizaciones | Con PDF, IVA opcional, estados |
| clientes | Clientes (CRM) | Base de datos de clientes |
| inventario | Inventario | Stock, alertas, categorías |
| finanzas | Finanzas | Ingresos, gastos, cuentas |
| produccion | Producción | Fabricación de productos propios (ej. Gasa Petrolada) |
| compras | Compras | Órdenes + tab Pagos a Proveedores |
| ventas | Ventas | Facturas + tab Cobros y Cuentas por Cobrar |
| contabilidad | Contabilidad | Libro diario, asientos |
| admin | Configuración | Usuarios, unidades de medida, empresa, backup |

### Estructura técnica clave
- `useReducer` centralizado con `dispatch` para todo el estado
- `AuthContext` provee `{canEdit, user, logout}` a todos los módulos
- `LoginScreen` — pantalla de login sin texto de roles visible
- `USERS` array — fuente de verdad de usuarios base
- `REGIMEN` — régimen fiscal Pequeño Contribuyente GT: IVA 5%, ISR 5%/7%
- Condiciones de pago en ventas: contado, crédito 15 días, crédito 30 días
- `RegistrarCobro` — componente modal para cobros de clientes
- `RegistrarPagoP` — componente modal para pagos a proveedores

### Reducer cases disponibles
ADD/UPDATE/DEL para: cotizaciones, clientes, productos, facturas, asientos, presupuestos, proyectos, pagosClientes, pagosProveedores. También: SET_UNIDADES, SET_CONFIG_EMPRESA, IMPORT_DATA.

### Validación de errores JSX
Usar TypeScript parser para detectar errores antes de abrir en navegador:
```bash
node -e "
const fs=require('fs');
const ts=require('/usr/local/lib/node_modules_global/lib/node_modules/typescript');
const src=fs.readFileSync('/sessions/blissful-wonderful-cerf/mnt/OHS/OHS_ERP.html','utf8');
const jsx=src.split('<script type=\"text/babel\">')[1].split('</script>')[0];
const sf=ts.createSourceFile('t.tsx',jsx,ts.ScriptTarget.Latest,true,ts.ScriptKind.TSX);
const d=sf.parseDiagnostics||[];
console.log('Errores:',d.length);
d.forEach(x=>console.log('Línea',sf.getLineAndCharacterOfPosition(x.start).line+1,x.messageText));
"
```

### Archivos adicionales
- `restaurar_cotizacion.html` — herramienta para restaurar cotización N° 20260410428, DISAR S.A., Q4,443.50 (47 ítems). Abrir en el mismo navegador del ERP y clic en "Restaurar".
- `setup_github.sh` — script bash para configurar repo GitHub desde cero.

