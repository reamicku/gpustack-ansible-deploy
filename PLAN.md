# Plan

**NIE RESETOWAĆ TYCH LAPTOPÓW, SĄ UPOŚLEDZONE I NIE WŁĄCZĄ SIĘ SPOWROTEM !!! BOOT ORDER JEST POMIESZANY**

~~systemctl reboot/poweroff~~

~~reboot/poweroff~~

## Hosty

| Host           | Rola                                                           |
| -------------- | -------------------------------------------------------------- |
| `172.16.3.190` | `gpustack-controller`, `gpustack-worker`, `monitor-controller` |
| `172.16.3.191` | `gpustack-worker`                                              |
| `172.16.3.192` | `gpustack-worker`                                              |
| `172.16.3.193` | `gpustack-worker`                                              |
| `172.16.3.194` | `gpustack-worker`                                              |

## Role

| Rola                  | Opis                                           |
| --------------------- | ---------------------------------------------- |
| `gpustack-controller` | Kontroler GPUStack z interfejsem do interakcji |
| `gpustack-worker`     | Pracownik GPUStack                             |
| `monitor-controller`  | Ma grafanę i prometeusza                       |
| `all`                 | Ma dockera i node-exportera                    |

## Deployment

1. `all` -- `apt update && apt upgrade`
1. `all` -- Instalacja podst. narzędzi
    * vim, htop, nvtop
1. `all` -- Instalacja dockera
    * Użyć oficjalne repo dockera dla debiana
    * Pewnie użyję docker.io z oficjanlego repo debiana bo mi się nie chce dodawać xD
1. `all` -- Instalacja node-exportera i konfiguracja
    1. Jako kontener dockera
1. `monitor-controller` -- Instalacja prometeusza, grafany i konfiguracja
    1. Jako kontener dockera
1. `gpustack-worker` -- Instalacja GPUStack i konfiguracja
1. `gpustack-controller` -- Instalacja GPUStack w trybie controller i konfiguracja

## Wyjście

- URL do interfejsu GPUStack
    * `http://172.16.3.190:8080`
- URL do interfejsu Grafany (do monitoringu)
    * `http://172.16.3.190:9000`