# Análise de Regressão Linear Múltipla - Produtividade Agrícola

## 📊 Objetivo
Analisar o impacto de fertilizantes, qualidade do solo e tipo de lavoura na produtividade agrícola usando regressão linear múltipla em R.

## Dataset
- **Variável Dependente:** Produtividade
- **Variáveis Independentes:** 
  - Fertilizantes (quantitativa)
  - Qualidade do Solo (quantitativa)
  - Lavoura (quantitativa)
- Fonte: Dados fictícios

## Passo a Passo da Análise

### 1. Preparação dos Dados
```r
library(readxl)
dados <- read_excel("Aula Lab 1.xlsx")
```

### 2. Modelagem Estatística
Modelo de regressão linear múltipla:
```r
modelo <- lm(Produtividade ~ Fertilizantes + Qualidade_solo + Lavoura, data = dados)
```

### 3. Interpretação do Modelo
Principais saídas do `summary(modelo)`:
- **Coeficientes:** Efeito de cada variável na produtividade
    - Coeficiente Angular (β0): -16.43062
    - Coeficiente Fertilizantes (β1): 0.82955
    - Coeficiente Qualidade do Solo (β2): 4.99885
    - Coeficiente Lavoura (β3): 0.19941
- **R-squared:** 0.9592 (Proporção da variabilidade explicada)
- **p-valores:** Significância estatística (p < 0.05 = significativo)
    - Coeficiente Angular: 7.55e-05 ***
    - Coeficiente Fertilizantes: 1.05e-06 ***
    - Coeficiente Qualidade do Solo: 0.0112 ***
    - Coeficiente Lavoura: 0.19941 *

### 4. Visualização Gráfica
Gráfico de dispersão com linha de tendência:
```r
ggplot(data = modelo, aes(y = Produtividade, x = fitted(modelo))) +
  geom_point(size = 2,
             alpha = 1,
             color = 'black',
             na.rm = TRUE) +
  geom_smooth(method = 'lm',
              color = '#01A77F',
              fill = 'pink',
              level = 0.95,
              se = TRUE,
              linewidth = 1.5) +
  labs(title = 'Impacto do uso de Fertilizantes na Produtividade Agrícola',
       subtitle = 'Desvendando os Drivers da Produtividade Agrícola',
       x = 'Fertilizantes (L/ha)',
       y = 'Produtivdade (Kg/m²)',
       caption = 'Fonte: Dados fictícios') +
  theme(panel.background = element_rect(fill = 'white'),
        plot.title = element_text(family = 'dosis', hjust = 0.5, face = 'bold', color = '#01A77F', size = 14),
        plot.subtitle = element_text(family = 'poppins', hjust = 0.5),
        plot.caption = element_text(family = 'poppins', hjust = 1, vjust = 18, color = 'black', size = 9),
        axis.title = element_text(family = 'poppins', hjust = 0.5, color = 'black', size = 9),
        axis.text = element_text(family = 'poppins', hjust = 0.5, color = 'black', size = 9),
        axis.line.x = element_line(),
        axis.line.y = element_line())
```

### 5. Pressupostos da Regressão
Verifique sempre:
1. Linearidade (gráfico resíduos vs ajustados)
```r
  ggplot(data = NULL, aes(x = fitted(modelo), y = residuals(modelo))) +
  geom_point(size = 2, colour = 'black') +
  geom_hline(yintercept = 0, linetype = "dashed") +
  labs(x = "Valores Ajustados", y = "Resíduos", 
       title = "Resíduos X Valores Ajustados") +
  theme(panel.background = element_rect(fill = 'white'),
        plot.title = element_text(family = 'dosis', colour = '#01A77F', hjust = 0.5, size = 14 , face = 'bold'),
        axis.title = element_text(family = 'poppins', hjust = 0.5, color = 'black'),
        axis.text = element_text(family = 'poppins', hjust = 0.5, color = 'black'),
        axis.line.x = element_line(),
        axis.line.y = element_line())
```
3. Homocedasticidade (Teste White)
```r
white(modelo, interactions = TRUE)
```
**Resultado:** Válido supor que há homocedasticidade
- Hipótese Nula (HO): Homocedasticidade
- X²: 12.4 com 9 graus de liberdade
- P-valor: (0.192 > 0.05)



4. Normalidade dos resíduos (Shapiro-Wilk)
5. Multicolinearidade (VIF)

## Resultados Esperados
- Equação de regressão:  
  `Produtividade = β0 + β1*Fertilizantes + β2*Qualidade_solo + β3*Lavoura`
- Relação quantitativa entre insumos agrícolas e produtividade
- Identificação de fatores mais impactantes
