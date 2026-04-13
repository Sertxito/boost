# Plantilla de Lista de Verificacion de Integracion FNMT

## Objetivo

Lista de verificacion de controles minimos para integracion de autenticacion/autorizacion basada en certificado FNMT.

## Validacion criptografica

- [ ] Cadena de confianza validada.
- [ ] Certificado dentro de vigencia.
- [ ] Revocacion verificada (OCSP/CRL).
- [ ] Politica de cache de estado de revocacion definida.

## Mapeo de identidad

- [ ] Regla de mapping certificado -> usuario interno documentada.
- [ ] Unicidad de identidad garantizada.
- [ ] Proceso de alta/baja/renovacion definido.

## Autorizacion

- [ ] Claims derivadas del certificado definidas.
- [ ] Politicas backend por rol/claim implementadas.
- [ ] Guardas frontend alineadas a permisos efectivos.

## Seguridad operativa

- [ ] Eventos de auditoria FNMT definidos.
- [ ] Alertas para fallos repetidos de validacion.
- [ ] Runbook de incidente de certificado comprometido.

## Cumplimiento y evidencia

- [ ] Evidencia de pruebas de extremo a extremo.
- [ ] Trazabilidad de decisiones de acceso.
- [ ] Responsable funcional y tecnico identificado.
