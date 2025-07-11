 🩺 Projeto 1: Visualizador e Analisador de Sinais de ECG

Este projeto é o primeiro módulo de uma jornada para aplicar Inteligência Artificial na área da saúde. O objetivo aqui é construir as fundações da análise de sinais, aprendendo a manipular, limpar e extrair informações valiosas de um sinal de Eletrocardiograma (ECG) bruto.

## 🎯 Objetivo

O programa carrega um sinal de ECG real do banco de dados do PhysioNet, remove os ruídos, detecta cada batimento cardíaco (picos R) e, finalmente, calcula a frequência cardíaca média em Batimentos Por Minuto (BPM).

## 🛠️ Ferramentas e Bibliotecas

* **Python 3.x**
* **WFDB:** Para carregar os dados e anotações diretamente do banco de dados do PhysioNet.
* **NumPy:** Para cálculos numéricos e manipulação de arrays de forma eficiente.
* **SciPy:** Para as tarefas de engenharia de sinais, especificamente a filtragem e a detecção de picos.
* **Matplotlib:** Para a visualização dos sinais em cada etapa do processo.
* **Jupyter Notebook (no VS Code):** Para um ambiente de desenvolvimento interativo.

## 📈 O Fluxo de Trabalho

O processo de análise foi dividido em 5 passos lógicos:

### 1. Carga dos Dados
O sinal utilizado foi o registro '100' do clássico banco de dados **MIT-BIH Arrhythmia Database**, obtido via PhysioNet. A biblioteca `wfdb` lida com o download e a leitura dos arquivos `.dat` (sinal) e `.hea` (cabeçalho).

### 2. Análise do Sinal Bruto
Uma primeira inspeção visual do sinal revela os batimentos cardíacos, mas também a presença de dois tipos principais de ruído que precisam ser tratados.

### 3. Filtragem (Pré-processamento) ⚡
Para limpar o sinal, um **filtro passa-faixa (band-pass) Butterworth** foi aplicado. Esse filtro tem duas missões:
* **Remover a oscilação da linha de base:** Um ruído de baixa frequência, provavelmente causado pela respiração do paciente.
* **Remover ruídos de alta frequência:** Como interferências da rede elétrica e ruído muscular.

O resultado é um sinal muito mais limpo, com os picos R proeminentes e a linha de base estável.

*(Dica: Substitua a imagem abaixo por um print do seu gráfico de "Sinal Bruto vs. Sinal Filtrado")*
![Comparação de Filtragem](https://i.imgur.com/8zU3iV5.png)


### 4. Detecção de Picos R
Com o sinal limpo, a função `find_peaks` da `SciPy` foi usada para localizar o índice de cada pico R. Para garantir a precisão, foram utilizadas duas regras:
* **Altura Mínima (`height`):** Os picos devem ter uma amplitude mínima para serem considerados, descartando ondas menores.
* **Distância Mínima (`distance`):** Dois picos não podem estar muito próximos, o que evita a detecção dupla no mesmo batimento (ex: onda R e onda T).

*(Dica: Substitua a imagem abaixo por um print do seu gráfico de "Detecção de Picos R")*
![Detecção de Picos](https://i.imgur.com/rM5J1jB.png)


### 5. Cálculo da Frequência Cardíaca (BPM) 算出
Com a localização exata de cada pico, os intervalos entre eles (intervalos RR) foram calculados. A partir da duração de cada intervalo em segundos, a frequência cardíaca instantânea e a média foram calculadas usando a fórmula `BPM = 60 / intervalo_em_segundos`.
