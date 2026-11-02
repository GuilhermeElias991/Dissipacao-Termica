# Balanço Térmico de Painel

Calculadora que soma as perdas de inversores PowerFlex 523, 525, 753 e 755 e recomenda a climatização Rittal do painel: convecção natural, ventilador com filtro, trocador de calor ar/ar ou unidade de refrigeração. O resultado vem com memorial de cálculo.

Site publicado: https://balanco-termico.vercel.app

## Como usar

Abra o site, ou o arquivo `index.html` no navegador, e informe:

1. os inversores, por modelo ou código de catálogo;
2. as dimensões e o tipo de instalação do gabinete;
3. as temperaturas, a altitude, a frequência e a condição do ar.

Não há etapa de build nem dependências. Todo o código e os dados estão em `index.html`, e os projetos digitados ficam só no navegador de quem usa.

## Como o cálculo funciona

- Área efetiva do gabinete pelos tipos de instalação da IEC 60890
- Dissipação pelas paredes: `Qs = A · k · ΔT`
- Vazão do ventilador: `V = f · Qd / ΔT`, com `f = 3,1` ao nível do mar, corrigido pela altitude
- Trocador de calor: `qw = (Qd − Qs) / ΔT`
- Refrigeração: `Qk = Qd − Qs`

## Limites conhecidos

Ferramenta de apoio ao dimensionamento. Confira os valores nas fichas do fabricante antes de especificar.

- As perdas do 753 e do 755 vêm da revisão B da publicação Rockwell 750-TD001, que é antiga, e cobrem só 400 V e 480 V em Normal Duty. Inversores de 600 V e 690 V e o 755T entram como "Outro inversor", com o valor do manual.
- As perdas do 523 e do 525 vêm do artigo 580997 da Rockwell (2019), que diverge da 520-TD001D nas linhas de 4 A a 480 V.
- Os coeficientes da IEC 60890 e os valores de k fora do aço não foram conferidos na norma.
- O ΔT mínimo de 5 K para usar ventilador é um critério de projeto desta ferramenta.

A lista completa e as fontes estão na seção "Dados e limites" da página.

Projeto independente, sem vínculo com a Rockwell Automation ou a Rittal.
