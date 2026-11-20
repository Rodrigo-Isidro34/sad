# 🚀 Kickstarter Intelligence — Data Mart & Dashboard Executivo (SAD)

**Autores:** Chrystian Eros e Rodrigo Isidro

Projeto completo de **Business Intelligence / Sistema de Apoio à Decisão (SAD)** para análise de campanhas de crowdfunding do **Kickstarter**, contemplando desde a extração, transformação e carga (**ETL no Pentaho Data Integration**) até a modelagem dimensional (**Star Schema em PostgreSQL**) e visualização executiva no **Power BI**.

---

## 📂 Estrutura do Repositório

```text
├── pentaho/                                    # Pipelines ETL do Pentaho Data Integration (Kettle)
│   ├── Job_Carga_DataMart.kjb                  # Job orquestrador da carga do Data Mart
│   ├── Dimensao_Categoria.ktr                  # Carga da dimensão Categoria
│   ├── Dimensao_Criador.ktr                    # Carga da dimensão Criador
│   ├── Dimensao_Localizacao.ktr                # Carga da dimensão Localização
│   ├── Dimensao_Projeto.ktr                    # Carga da dimensão Projeto
│   ├── Dimensao_Tempo.ktr                      # Carga da dimensão Tempo
│   └── Tabela_Fato.ktr                         # Carga da tabela fato (Projetos_kickstarter)
├── powerbi/                                    # Dashboard Power BI
│   └── Projeto.pbix                            # Arquivo único do Power BI Desktop (modelo + relatório)
├── docs/                                       # Apresentação e evidências visuais
│   ├── Chrystian Eros e Rodrigo Isidro.pptx    # Apresentação executiva do projeto
│   └── prints/                                 # Capturas de tela do ETL (pentaho/) e do Dashboard (dashboard/)
└── README.md                                   # Documentação do repositório
```

> **Nota sobre o dataset bruto:**
> O arquivo CSV original (`Kickstarter Full Campaigns_694062_25_April_2026.csv`, ~454 MB) não está versionado neste repositório por exceder o limite de tamanho de arquivos do GitHub (100 MB).

---

## 🏗️ Arquitetura da Solução

### 1. Pipeline ETL (Pentaho Data Integration)
O fluxo orquestrado pelo arquivo [`pentaho/Job_Carga_DataMart.kjb`](./pentaho/Job_Carga_DataMart.kjb) processa a base bruta do Kickstarter e popula o Data Mart no **PostgreSQL** (conexão `DW_Kickstarter`, banco `SAD`):
- **Dimensões:** `Categoria`, `Criador`, `Localizacao`, `Projeto` e `Tempo`
- **Tabela Fato:** `Projetos_kickstarter` (métricas de arrecadação em USD, meta financeira, quantidade de apoiadores, duração e flag de sucesso)

### 2. Modelagem Dimensional e Dashboard (`powerbi/Projeto.pbix`)
- Arquitetura **Star Schema** com tabela centralizadora `_Medidas` contendo:
  - **01. KPIs Principais:** Total Arrecadado (USD), Capital Liquidado (*All-or-Nothing*), Meta Total, Total de Apoiadores, Taxa de Sucesso (%), Ticket Médio por Apoiador, Duração Média.
  - **02. Inteligência Temporal:** Comparativos YoY (Year-over-Year), MoM (Month-over-Month) e acumulados.
  - **Parâmetros Dinâmicos:** Seleção dinâmica de métricas (`Parâmetro de Métrica`) e dimensões (`Parâmetro de Dimensão`).
- **Páginas do Dashboard:**
  1. **00. Início:** Capa e navegação executiva.
  2. **01. Panorama:** Visão consolidada dos KPIs globais e evolução temporal de captação.
  3. **02. Anatomia:** Fatores determinantes de sucesso, metas, categorias e comportamento de campanha.
  4. **03. Geografia:** Distribuição geográfica de projetos, arrecadação e apoiadores.
  5. **04. Raio-X:** Detalhamento granular por campanha e criador.

---

## ▶️ Como Executar

1. **Banco de Dados & ETL:**
   - Crie um banco PostgreSQL local chamado `SAD` (porta `5432`).
   - Abra o [`pentaho/Job_Carga_DataMart.kjb`](./pentaho/Job_Carga_DataMart.kjb) no **Pentaho Data Integration (Spoon)**, aponte o arquivo CSV de origem das campanhas e execute o Job.
2. **Power BI:**
   - Abra o arquivo [`powerbi/Projeto.pbix`](./powerbi/Projeto.pbix) no **Power BI Desktop**.
   - Atualize a conexão com o banco PostgreSQL local caso necessário.
