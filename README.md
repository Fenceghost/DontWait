# Lantern (Lampião) – Don’t Wait

Este script controla o lampião da personagem principal (Ayla) em **Don’t Wait**, incluindo:

- Luz dinâmica (flicker / agitação)
- Durabilidade e estados (forte, normal, fraco, apagado)
- Interferência química da criatura (espasmos)
- Influência do ambiente (umidade reduz o alcance da luz)
- Partículas de fumaça reativas
- Som reativo à agitação da lanterna
- LightOccluder2D reativo

---

## **Signals**
- `durability_changed(value: float)` – Disparado quando a durabilidade muda.
- `state_changed(state: int)` – Disparado quando o estado da lanterna muda.
- `lantern_extinguished` – Disparado quando a lanterna apaga.
- `threat_level_changed(level: float)` – Disparado quando a agitação da criatura muda.
- `chemical_spasm_triggered` – Disparado quando a criatura provoca espasmo químico.
- `chemical_spasm_ended` – Disparado quando o espasmo termina.

---

## **Estados da Lanterna**
- `STRONG` – Luz forte.
- `NORMAL` – Luz normal.
- `WEAK` – Luz fraca.
- `RETREAT` – (Opcional, futuro) Luz recuando.
- `OFF` – Lanterna apagada.

---

## **Exported Properties**

**Durability**
- `max_durability` – Durabilidade máxima da lanterna.
- `drain_rate` – Taxa de consumo de durabilidade por segundo.

**Light Energy**
- `strong_energy`, `normal_energy`, `weak_energy` – Intensidade da luz em cada estado.

**Light Radius**
- `base_radius` – Raio base da lanterna.
- `humidity_radius_penalty` – Penalidade de raio em ambientes úmidos.

**Idle Flicker / Agitation**
- `idle_flicker_speed`, `idle_flicker_strength` – Velocidade e intensidade do flicker natural.
- `agitation_speed_multiplier`, `agitation_strength_multiplier` – Modificadores quando a criatura está próxima.

**Chemical Interference**
- `spasm_chance` – Chance de espasmo químico por segundo.
- `spasm_duration` – Duração do espasmo.
- `spasm_energy_drop` – Redução de energia durante espasmo.

**Audio**
- `calm_volume` – Volume da lanterna em estado calmo.
- `stress_volume` – Volume da lanterna em estado estressado.

**Particles**
- `smoke_base_amount` – Quantidade mínima de partículas.
- `smoke_max_amount` – Quantidade máxima de partículas.

---

## **Métodos Principais**

- `_consume_durability(delta)` – Reduz durabilidade da lanterna.
- `_update_state()` – Atualiza o estado da lanterna de acordo com durabilidade.
- `_update_light_immediate()` – Ajusta energia da luz sem flicker.
- `_apply_flicker()` – Aplica flicker natural e agitação.
- `_process_chemical_interference(delta)` – Controla espasmos químicos da criatura.
- `_update_audio()` – Ajusta sons da lanterna conforme agitação.
- `_update_particles()` – Controla partículas de fumaça.
- `_update_occluders()` – Atualiza sombras dos LightOccluder2D.
- `_update_light_radius()` – Atualiza raio da lanterna conforme umidade.

---

## **Interface Externa**
- `set_threat_level(value: float)` – Define o nível de ameaça da criatura (0 a 1).
- `set_environment_humidity(value: float)` – Define a umidade do ambiente (0 a 1).

---

## **Como usar na cena**
1. Crie um nó **Node2D** chamado `Lantern`.
2. Adicione os seguintes nós filhos:
   - `PointLight2D` → `Light`
   - `AudioStreamPlayer2D` → `AudioIdle`
   - `AudioStreamPlayer2D` → `AudioStress`
   - `AudioStreamPlayer2D` → `AudioSpasm`
   - `GPUParticles2D` → `SmokeParticles`
3. Configure a luz, áudio e partículas conforme desejado.
4. Adicione este script ao nó `Lantern`.
5. Adicione LightOccluder2D em objetos do cenário e coloque-os no grupo `lantern_occluders`.

---


## **Dicas Profissionais**
- Use `signals` para comunicar mudanças de estado para HUD ou Player.
- Ajuste `agitation` externamente via `set_threat_level()` para sincronizar efeitos de luz e som.
- Mantenha partículas leves (`smoke_max_amount` baixo) para performance em PC e mobile.
- Ajuste LightOccluder2D dinamicamente para efeitos de sombras realistas.


# Player – Don’t Wait

Este script controla o jogador no estilo **4-direções**, com:

- Movimento com caminhada e corrida.
- Sistema de stamina, impedindo corrida com energia baixa.
- Passos dinâmicos com variação de pitch e volume baseada na lanterna e stamina.
- Diferenciação de sons de passos por tipo de superfície: madeira, pedra e lama.
- Integração com o lampião (Lantern) para irregularidade e tremor de passos.
- Troca de animações baseada em movimento, stamina e agitação do lampião.

---

## **Exported Properties**

**Movement**
- `speed_walk` – Velocidade de caminhada.
- `speed_run` – Velocidade de corrida.

**Stamina**
- `max_stamina` – Energia máxima.
- `stamina_drain_rate` – Consumo de stamina por segundo ao correr.
- `stamina_recover_rate` – Recuperação de stamina por segundo ao andar.
- `stamina_min_run` – Mínimo de stamina para poder correr.

**Steps**
- `step_interval_walk` – Intervalo base de passos caminhando.
- `step_interval_run` – Intervalo base de passos correndo.

**TileMap**
- `ground_tilemap_path` – Caminho para o TileMap do chão.
- `step_sounds_wood`, `step_sounds_stone`, `step_sounds_mud` – Arrays de sons de passos por superfície.

---


## **Dicas de Implementação**

- Configure as tiles do TileMap com a propriedade `surface_type` para reproduzir o som correto.
- Ajuste `step_interval_walk` e `step_interval_run` conforme ritmo do jogo.
- `Lantern.agitation` afeta ritmo e pitch dos passos para sensações de tensão.
- Use `stamina` para definir animações de cansaço.
- `signals` do lampião podem ser usados para HUD ou efeitos visuais.

