# Balanço Térmico, Dimensionamento de Cabos e Energia Incidente

O site tem três ferramentas, com abas no topo para alternar entre elas:

- **Balanço térmico** (`index.html`): perdas de inversores e climatização do painel.
- **Dimensionamento de cabos** (`cabos.html`): seção de cabos de baixa tensão pela NBR 5410, em CA ou CC.
- **Energia incidente** (`energia.html`): arco elétrico pela IEEE 1584-2018, com relatório em PDF.

## Balanço térmico

Calculadora que soma as perdas de inversores PowerFlex 523, 525, 753 e 755 e recomenda a climatização Rittal do painel: convecção natural, ventilador com filtro, trocador de calor ar/ar ou unidade de refrigeração. O resultado vem com memorial de cálculo.

Site publicado: https://balanco-termico.vercel.app

## Como usar

Abra o site, ou o arquivo `index.html` no navegador, e informe:

1. os inversores, por modelo ou código de catálogo;
2. as dimensões e o tipo de instalação do gabinete;
3. as temperaturas, a altitude, a frequência e a condição do ar.

Não há etapa de build nem dependências. Cada ferramenta tem o código e os dados no próprio HTML, e os projetos digitados ficam só no navegador de quem usa.

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

## Dimensionamento de cabos

Abra `cabos.html` (ou a aba "Dimensionamento de cabos") e informe:

1. o circuito: corrente contínua, CA monofásico, bifásico, trifásico ou trifásico com neutro; tensão; carga em A, kW, kVA ou potência no eixo de motor (CV ou kW); fator de potência; comprimento e queda máxima;
2. o cabo: cobre ou alumínio, PVC ou EPR/XLPE, e o método de instalação da Tabela 33, com o esquema de cada método;
3. a temperatura ambiente (ou do solo) e o agrupamento;
4. o condutor de proteção, o neutro e o eletroduto.

O cálculo:

- Corrente de projeto: `Ib = P / V` (CC e monofásico) ou `Ib = P / (√3 · V · FP)` (trifásico)
- Fatores: FCT pela Tabela 40 e FCA pela Tabela 42
- Capacidade: menor seção com `Iz · FCT · FCA ≥ Ib` (ou `≥ In`, se o disjuntor for informado), pelas Tabelas 36 a 39
- Queda de tensão em CA: `ΔV = u · Ib · L`, com `u` em V/A·km da Tabela 19 da Pirelli, interpolado entre FP 0,8 e 0,95
- Queda de tensão em CC: `ΔV = 2 · ρθ · L · Ib / S`
- Seção adotada: a maior entre capacidade, queda de tensão e seção mínima (Tabela 47)
- Neutro pela Tabela 48, condutor de proteção pela Tabela 58 e eletroduto pelas Tabelas A a E

Os esquemas dos métodos de instalação e as figuras de referência ficam em `img/`, recortados do material de apoio.

Limites conhecidos:

- As tabelas foram transcritas da reprodução da NBR 5410:2004 do material de apoio. A linha de cobre 300 mm² da Tabela 39 (método E, 3 condutores) traz 261 A no material, e a página usa 621 A.
- A Tabela 19 é só para cobre de 1,5 mm² a 500 mm². Fora dela, e para alumínio, a queda usa a resistividade e uma reatância típica de 0,10 Ω/km.
- Para cabos enterrados (método D), o fator de agrupamento deve ser informado à mão: as tabelas próprias da norma não estão no material.
- Curto-circuito (I²t) não é verificado.

Projeto independente, sem vínculo com a Rockwell Automation ou a Rittal.

## Energia incidente

Abra `energia.html` (ou a aba "Energia incidente") e informe a identificação do relatório, a tensão, o curto-circuito, a configuração dos eletrodos (VCB, VCBB, HCB, VOA ou HOA), as distâncias G e D, os tempos de eliminação do arco e as dimensões do invólucro.

A página segue a IEEE 1584-2018: corrente de arco a 600 V, 2700 V e 14300 V com interpolação, corrente mínima pelo fator de variação, fator de correção do invólucro, energia incidente e limite de aproximação nos dois cenários (Ia com T e Ia mínima com Tmin), valendo o pior. A categoria e o ATPV seguem as faixas da NFPA 70E.

Conferência: com os dados do relatório de exemplo do QTei2018 (VCB, 30 kA, 380 V, G 25 mm, D 457 mm, 100 ms, 2000 × 2000 × 800 mm), a página dá Ia = 19,517 kA e E = 14,979 J/cm² (3,58 cal/cm²), iguais aos do relatório, e AFB = 905,7 mm contra 905,675 mm.

O botão "Gerar PDF" monta o relatório no próprio navegador, com capa, cabeçalho e rodapé da JAV (`img/jav/`), usando a biblioteca jsPDF guardada em `vendor/jspdf/` (licença MIT). Os dados da empresa no rodapé ficam na constante `COMPANY` de `energia.html`. O PDF só é gerado com a página aberta pelo site, e não como arquivo local.
