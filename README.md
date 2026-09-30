# Homelab - Red y DNS Interno

Este repositorio documenta, de forma sanitizada, el diseno de red de un
homelab personal: segmentacion por funcion dentro de un unico hipervisor,
DNS interno centralizado, y un caso real donde la propia segmentacion bloqueo
un flujo de monitoreo legitimo.

Es parte de un portfolio tecnico pensado para entrevistas de trabajo. No es
documentacion operativa de un entorno en produccion: es una version
transformada -decisiones, patrones y aprendizajes- de un homelab real, sin
datos que permitan identificarlo o reproducirlo.

Lo que busca demostrar: criterio para segmentar una red domestica por
funcion cuando la red fisica no da soporte nativo a esa segmentacion,
capacidad de razonar sobre DNS interno como dependencia critica, y el
habito de tratar cada excepcion a una regla de segmentacion como algo que se
documenta y se justifica, no como un atajo silencioso.

## Indice

- [Arquitectura de red y segmentacion](docs/01-arquitectura-red-segmentacion.md)
- [Caso de estudio: monitoreo bloqueado por segmentacion](docs/casos-de-estudio/01-monitoreo-bloqueado-por-segmentacion.md)

## Licencia

Ver [LICENSE.md](LICENSE.md).
