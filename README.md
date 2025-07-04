# homeassistant_estado_predeterminado_luces
## 🛡️ Automatización silenciosa que corrige luces al blanco si se encienden sin color definido.

Hay muchas luces de color que no tienen estado predeterminado, por lo que si las encendemos en rojo, al día siguiente seguirán en rojo. Lo que hace esta automatización es establecer el estado predeterminado al encender manualmente las luces, tanto desde Alexa, como desde el propio interruptor físico.

Haremos que las luces vuelvan al estado deseado (blanco, 80 % de brillo) sin parpadeos ni inconsistencias:

### 🧠 1. Automatización que intercepta la orden de encendido

Puedes crear una automatización que detecte cuándo una luz se enciende sin color definido, y reestablece tus parámetros.

### 🛡️ Automatización silenciosa que corrige luces al blanco si se encienden sin color definido:

```
alias: Restaurar estado blanco al encender
trigger:
  - platform: state
    entity_id:
      - light.lampara_susana
      - light.lampara_pedro
      - light.led_dormitorio
    to: "on"
condition:
  - condition: template
    value_template: >
      {{ state_attr(trigger.entity_id, 'color_temp') == None }}
action:
  - service: light.turn_on
    target:
      entity_id: "{{ trigger.entity_id }}"
    data:
      color_temp: 250
      brightness_pct: 80
mode: queued
```

🪄 Esto hace magia silenciosa: si alguien enciende la luz desde Alexa o el panel sin definir color, el sistema lo corrige.
