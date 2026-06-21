# FIELDFLOW
ROUTING, REPORTE E CRM DE EQUIPAS DE TERRENO

## Mecânica de atribuição de rotas por distância

Para melhorar a atribuição de rotas por distância, a regra recomendada é:

1. Calcular a distância em km por **tempo real de estrada** (API de routing); se indisponível, usar Haversine como fallback.
2. Ordenar candidatos por menor distância.
3. Em empate, priorizar quem tem menos pontos já atribuídos.
4. Em novo empate, usar maior densidade geográfica da rota (menos deslocações totais).

### Score sugerido

`score = (0.7 * proximidade_normalizada) + (0.2 * balanceamento_carga) + (0.1 * densidade_rota)`

- **proximidade_normalizada**: maior valor para menor distância.
- **balanceamento_carga**: maior valor para colaborador com menos visitas.
- **densidade_rota**: maior valor para rota com menor dispersão.

### Normalização (escala 0–1)

- `proximidade_normalizada = 1 - ((dist_atual - dist_min) / (dist_max - dist_min))`
- `balanceamento_carga = 1 - ((carga_atual - carga_min) / (carga_max - carga_min))`
- `densidade_rota = 1 - ((dispersao_atual - dispersao_min) / (dispersao_max - dispersao_min))`

Quando `max == min`, usar `1` para evitar divisão por zero.

Isto reduz deslocações desnecessárias e melhora o equilíbrio operacional.
