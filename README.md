# Gastos do Cartão de Pagamento do Governo Federal (CPGF): Jun–Ago/2026

Análise com **Python e pandas** dos gastos com cartão corporativo do governo federal, a partir dos dados abertos do Portal da Transparência. O projeto reúne três meses de extratos em um único conjunto de dados, faz a limpeza, e mostra os gastos por órgão superior (ministério) mês a mês, com a variação percentual entre os meses.

> English summary at the [end of this file](#english-summary).

## Resumo dos resultados

| Mês (extrato) | Total gasto | Variação |
|---|---|---|
| Junho/2026 | R$ 8.685.571,94 | n/a |
| Julho/2026 | R$ 7.549.570,10 | −13,08% |
| Agosto/2026 | R$ 8.194.871,92 | +8,55% |
| **Trimestre** | **R$ 24.430.013,96** | |

**Os cinco órgãos que mais gastaram no trimestre:**

| # | Órgão superior | Total no trimestre | % do total |
|---|---|---|---|
| 1 | Ministério da Defesa | R$ 5.228.004,76 | 21,4% |
| 2 | Ministério da Educação | R$ 4.620.244,05 | 18,9% |
| 3 | Presidência da República | R$ 3.280.609,35 | 13,4% |
| 4 | Ministério da Justiça e Segurança Pública | R$ 2.949.040,90 | 12,1% |
| 5 | Ministério do Planejamento e Orçamento | R$ 2.214.025,09 | 9,1% |

- Os três maiores órgãos concentram cerca de **54%** dos gastos, e os cinco maiores, cerca de **75%**.
- O total caiu em julho e subiu em agosto, mas sem retornar ao nível de junho.
- Alguns órgãos têm variações percentuais muito altas porque partem de valores pequenos (por exemplo, a Previdência Social, com +293% de junho para julho). Por isso a tabela mostra o valor em reais junto com a variação.

## Fonte dos dados

- **Portal da Transparência: Cartão de Pagamento do Governo Federal (CPGF)**: https://portaldatransparencia.gov.br/download-de-dados/cpgf
- Arquivos usados: `202606_CPGF.csv`, `202607_CPGF.csv` e `202608_CPGF.csv`.
- A coluna `MÊS EXTRATO` indica o mês do extrato (fatura), e não necessariamente o mês em que a compra foi feita. Por isso, extratos de junho podem conter compras com datas de abril e maio.

Os arquivos de dados **não estão incluídos neste repositório**. Para reproduzir, baixe os três arquivos no link acima e coloque-os na pasta `dados/`.

## O que o projeto faz

1. **Carrega e junta** os três CSVs em um único DataFrame (`pathlib` e list comprehension, funcionando em Windows e Linux/macOS).
2. **Remove linhas duplicadas** (`drop_duplicates`).
3. **Converte** o campo `VALOR TRANSAÇÃO` de texto para número (formato brasileiro `1.594,78` → `1594.78`).
4. **Gera uma tabela pivotada** por órgão superior, com o gasto de cada mês, a variação percentual entre meses e o total do trimestre, ordenada do maior para o menor gasto.

## Decisões de tratamento dos dados

- **Duplicatas:** foram removidas **11.451 linhas** idênticas. O arquivo do CPGF não traz um identificador único do lançamento, então duas compras reais com mesmo valor, mesmo favorecido, mesmo portador e mesma data não podem ser distinguidas de uma repetição. A remoção foi uma decisão de projeto, e o resultado pode subestimar os gastos reais.
- **Valores nulos:** correspondem a informações sigilosas (por exemplo, dados de portador e favorecido protegidos). Foram **mantidos**, porque a análise usa o valor da transação, e não esses campos.
- **Variação percentual:** calculada como `(mês novo − mês anterior) ÷ mês anterior`. Quando o mês anterior é zero, a variação não é definida e é exibida como 0,00%, o que **não** significa "sem variação".
- **Nomes de órgãos truncados:** alguns nomes já chegam cortados no arquivo original (por exemplo, "Ministério do Meio Ambiente e Mudança do Cl") e foram mantidos como vieram.

## Como executar

```bash
git clone https://github.com/martinsrafa007
cd Gastos-do-Cart-o-de-Pagamento-do-Governo-Federal-CPGF-Jun-Ago-2026

python -m venv venv
# Windows: .venv\Scripts\activate
# Linux/macOS: source .venv/bin/activate

pip install -r requirements.txt
```

Baixe os três CSVs, coloque na pasta `dados/` e abra o `main.ipynb` (Jupyter ou VS Code), executando as células em ordem.

## Estrutura do repositório

```
.
├── main.ipynb          # Notebook com a análise completa
├── requirements.txt    # Dependências
├── dados/              # CSVs do Portal da Transparência (não versionados)
└── README.md
```

## Tecnologias

Python · pandas · NumPy · Jupyter Notebook

## Próximos passos

- [ ] Gráfico de barras com os 10 órgãos que mais gastaram
- [ ] Gráfico da evolução mensal do total
- [ ] Análise por tipo de transação e por favorecido
- [ ] Tratar a variação indefinida como valor ausente em vez de 0,00%
- [ ] Exportar a tabela final para Excel

---

## English summary

Analysis of Brazilian federal government corporate card spending (CPGF) for June–August 2026, using open data from the Transparency Portal. Three monthly CSV files are merged into a single pandas DataFrame, cleaned (duplicates removed, currency strings converted to floats), and aggregated by top-level agency with month-over-month percentage change.

**Key findings:** total spending was R$ 24.43 million over the quarter. The three largest agencies (Defense, Education, and the Presidency) account for about 54% of the total, and the top five for about 75%. Total spending fell 13.1% from June to July and rose 8.5% from July to August.

**Stack:** Python, pandas, NumPy, Jupyter Notebook.
