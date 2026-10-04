# Contexto - network-segmentation-playbook

Ficha de lectura rapida: que es, por que existe y que muestra.

## 1. Que es

Segmentacion por funcion dentro de un unico hipervisor y DNS interno centralizado como dependencia critica.

## 2. Por que existe

Mostrar criterio, no codigo: que problema habia, por que importaba, que se
decidio y por que, que salio mal en el camino y como se resolvio. El entorno es
infraestructura productiva -chica, pero con todas sus piezas y funcionando
24/7-; su documentacion es privada y este repo es su version transformada.

## 3. Que muestra

- Segmentar sin soporte nativo de la red fisica
- DNS interno como dependencia de todo lo demas
- Excepciones de segmentacion documentadas y justificadas

## 4. Ficha

| Item | Valor |
| --- | --- |
| Formato | Markdown y diagramas Mermaid, sin codigo |
| Casos de estudio | 1 (monitoreo legitimo bloqueado por la propia segmentacion), mas un documento de resultados medidos |
| Perfil al que apunta | Redes, infraestructura, seguridad |
| Estado | Completo, se amplia con casos nuevos |

Ultima actualizacion: 2026-10-04 (criterio del portfolio).
