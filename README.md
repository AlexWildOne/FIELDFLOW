# FIELDFLOW
ROUTING, REPORTE E CRM DE EQUIPAS DE TERRENO

## Mecânica de atribuição de rotas por distância

Para melhorar a atribuição de rotas por distância, a regra recomendada é:

1. Calcular a **distância real de estrada em km** (API de routing); se indisponível, usar Haversine como fallback.
2. Ordenar candidatos por menor distância.
3. Em empate, priorizar quem tem menos pontos já atribuídos.
4. Em novo empate, usar maior densidade geográfica da rota (menos deslocações totais).

### Score sugerido

`score = (0.7 * proximidade_normalizada) + (0.2 * balanceamento_carga) + (0.1 * densidade_rota)`

- **proximidade_normalizada**: maior valor para menor distância.
- **balanceamento_carga**: maior valor para colaborador com menos visitas.
- **densidade_rota**: maior valor para rota com menor dispersão.

### Normalização (escala 0–1)

Para cada métrica, usar:

`normalizado = 1`, quando `max == min`

`normalizado = 1 - ((valor_atual - valor_min) / (valor_max - valor_min))`, quando `max != min`

Aplicar a mesma lógica para proximidade, carga e dispersão, evitando divisão por zero.

Isto reduz deslocações desnecessárias e melhora o equilíbrio operacional.
