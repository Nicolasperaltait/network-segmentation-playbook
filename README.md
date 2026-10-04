# Network Segmentation Playbook

> Segmentar por funcion, tratar el DNS como dependencia critica y justificar cada excepcion.

<p align="center">
  <img src="https://img.shields.io/badge/Segmentacion-1F2937?style=for-the-badge&logo=proxmox&logoColor=white" alt="Segmentacion" />
  <img src="https://img.shields.io/badge/DNS_interno-B91C1C?style=for-the-badge&logo=pihole&logoColor=white" alt="DNS interno" />
  <img src="https://img.shields.io/badge/Firewall_por_host-111827?style=for-the-badge&logo=linux&logoColor=white" alt="Firewall por host" />
  <img src="https://img.shields.io/badge/13.017_frenados-242424?style=for-the-badge&logo=tailscale&logoColor=white" alt="13.017 frenados" />
  <img src="https://img.shields.io/badge/Resultados_medidos-F46800?style=for-the-badge&logo=grafana&logoColor=white" alt="Resultados medidos" />
</p>

Este repositorio documenta, de forma sanitizada, el diseno de red de una infraestructura productiva personal (homelab): segmentacion por funcion dentro de un unico hipervisor,
DNS interno centralizado, y un caso real donde la propia segmentacion bloqueo
un flujo de monitoreo legitimo.

Es parte de un portfolio tecnico. **No es un laboratorio de prueba: es
infraestructura productiva.** No tiene la escala de una empresa, pero tiene
todas sus piezas -virtualizacion, red segmentada, DNS, almacenamiento, backups
con copia externa, monitoreo, SIEM, acceso remoto y aplicaciones en uso- y
funciona 24/7 sobre un hipervisor de tipo 1 (Proxmox VE) en un servidor dedicado. Cuando
algo falla, el impacto es real.

La documentacion operativa es privada. Esto es su version transformada
-decisiones, patrones y aprendizajes-, sin datos que permitan identificar o
reproducir el entorno.

Lo que busca demostrar: criterio para segmentar una red por funcion cuando
el equipamiento fisico no da soporte nativo a esa segmentacion,
capacidad de razonar sobre DNS interno como dependencia critica, y el
habito de tratar cada excepcion a una regla de segmentacion como algo que se
documenta y se justifica, no como un atajo silencioso.

## Escala chica, exigencia de produccion

| Pieza | Con que | Si falla |
|---|---|---|
| Virtualizacion | Proxmox VE, hipervisor de tipo 1; una maquina por funcion | cae todo lo demas |
| DNS interno | Pi-hole, resolucion para todos los equipos y servicios | todo parece caido aunque este sano |
| Red y acceso remoto | zonas por funcion; Tailscale sin puertos abiertos, politica por puerto | se pierde el aislamiento o el acceso desde afuera |
| Almacenamiento y backups | OpenMediaVault, backups nocturnos, copia cifrada externa, pruebas de restauracion | se pierde la capacidad de recuperar |
| Monitoreo y seguridad | Prometheus, Grafana, Wazuh y alertas al telefono | los incidentes pasan sin que nadie se entere |
| Aplicaciones propias | Docker detras de Nginx Proxy Manager; una envia correo real | se frena trabajo real |
| Codigo | Forgejo privado con integracion continua | no hay donde versionar ni desde donde desplegar |

Lo mismo que en una empresa, en chico: cambios con plan y rollback, evidencia,
alertas que avisan solas y controles que se prueban haciendolos fallar.

## En 30 segundos

| Indicador | Resultado |
|---|---|
| Puertos entrantes abiertos en el borde | **0** |
| Intentos no autorizados frenados por la politica en un solo incidente | **13.017** en 34 horas |
| Paquetes descartados por politica en los hosts que filtran | **19.256** |
| Superficie de gestion cerrada en el router | Telnet y web desde Wi-Fi, FTP, UPnP |
| Reglas de firewall muertas retiradas | **6** |

```mermaid
flowchart LR
    I((Internet)) -.->|0 puertos entrantes| B[Router de borde]
    B --> ADM[Administracion]
    ADM -->|flujos minimos| SRV[Servicios]
    ADM -->|flujos minimos| SEC[Seguridad]
    SRV -->|solo metricas| SEC
    M[Malla de acceso remoto] -->|politica por puerto| ADM
```

Detalle con cifras: [Resultados medidos](docs/02-resultados-medidos.md).

## En vivo

_Capturas reales del entorno, con nombres, direcciones, usuarios y versiones reemplazados por su funcion._

![Pi-hole con consultas y bloqueos de las ultimas 24 horas](docs/img/pihole-dashboard.png)
<sub>Pi-hole en 24 horas: 133.123 consultas, 5.619 bloqueadas (4,2 %) y 74.761 dominios en listas.</sub>

## Problema, decision, resultado

| Problema | Por que importaba | Que se hizo | Resultado |
|---|---|---|---|
| Un flujo de monitoreo legitimo quedaba bloqueado por la segmentacion | abrir la zona entera rompia el modelo | permitir solo colector hacia exportador, documentado | monitoreo restaurado sin lateralidad |
| El router traia Telnet accesible desde Wi-Fi y UPnP con un puerto huerfano | credenciales en texto plano y puertos sin dueno | endurecimiento completo del borde | 0 puertos entrantes, gestion solo por cable |
| Se iba a instalar un firewall nuevo sin saber que filtraba cada host | trabajo duplicado y riesgo de romper lo que andaba | relevar primero, con contadores reales | plan corregido, 6 reglas muertas retiradas |

El detalle de cada uno, con lo que salio mal en el camino, esta en los casos de estudio.

## Indice

- [Ficha rapida para quien evalua](contexto.md)
- [Arquitectura de red y segmentacion](docs/01-arquitectura-red-segmentacion.md)
- [Resultados medidos: que se filtro, que no y que se previno](docs/02-resultados-medidos.md)
- [Caso de estudio: monitoreo bloqueado por segmentacion](docs/casos-de-estudio/01-monitoreo-bloqueado-por-segmentacion.md)

## Parte de una serie

Este repo es una pieza de **[Homelab Prod](https://github.com/Nicolasperaltait/homelab)**:
la vista completa de una infraestructura productiva, chica en escala y completa
en piezas, encendida 24/7. Cada repo de la serie se lee solo; la portada los une.

- [Zero Trust Remote Access](https://github.com/Nicolasperaltait/zero-trust-remote-access)
- [Network Segmentation Playbook](https://github.com/Nicolasperaltait/network-segmentation-playbook) (este repo)
- [Alerts That Matter](https://github.com/Nicolasperaltait/alerts-that-matter)
- [Backups That Don't Lie](https://github.com/Nicolasperaltait/backups-that-dont-lie)
- [Hypervisor as Control Plane](https://github.com/Nicolasperaltait/hypervisor-as-control-plane)
- [SecOps Governance Blueprint](https://github.com/Nicolasperaltait/secops-governance-blueprint)

## Licencia

Ver [LICENSE.md](LICENSE.md).
