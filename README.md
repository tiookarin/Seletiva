Repositório de soluções # HackaQuantum
Discente: João Victor Neves de Souza Nunes
A maior parte dos algoritmos utilizados serão retirados do Guia "A Practical Guide to Quantum Machine Learning and Quantum Optimization  - Hands-on Approach to Modern Quantum Algorithms" da minha experiência na área de QML.
HackaQuantum - O HackaQuantum faz parte do II Simpósio da LACIQ, evento realizado para comemorar o Ano Internacional da Quântica (2025).

Introdução:

# 🌌 Quantum-Robotics-Hybrid-Engine

> Hybrid Intelligence for Autonomous Systems

## 📜 Sobre o Projeto
Este é um framework experimental projetado para resolver problemas computacionais clássicos em robótica autônoma (Percepção e Sensoriamento, Planejamento e mapeamento, Controle e Dinâmica, Segurança e Sistemas) utilizando Quantum Machine Learning (QML). Focado na era NISQ (Noisy Intermediate-Scale Quantum) e além, este repositório implementa pipelines híbridos onde a CPU/GPU gerencia o pré-processamento e otimização, enquanto a QPU (Quantum Processing Unit) simulalda resolve tarefas de alta complexidade dimensional.

Minha missão é converter problemas NP-difíceis de robótica (Path Planning, SLAM, Cinemática Inversa,Funcionais) em:

Circuitos Variacionais (VQC) para controle e classificação.
Quantum-Enhanced Bayesian Inference para Fusão de Sensores
Quantum Kernels para percepção em ambientes ruidosos.
Recursive QAOA (R-QAOA) para otimização combinatória de trajetórias com Decomposição QUBO.
Otimização de Alocação de Tarefas (MRTA) via QGNN.
Aceleração de SLAM via VQLS.
Planejamento de Movimento em Espaços Apertados via QBM.
Quantum Tensor Networks (QTN)
Quantum Topological Data Analysis (QTDA)
Quantum Random Walks (QRW)
Hybrid Quantum MPC
Quantum Reservoir Computing (QRC)
Quantum Anomaly Detection via Density Matrices
Blind Quantum Computing (BQC)
Entre outros métodos. . .

Desafios:
Trilha: Percepção & Sensoriamento

Foco: Lidar com ruído, classificação de dados não estruturados e séries temporais.

A1 — Tipo de Terreno via LiDAR Minúsculo (Fácil)

    O Desafio Clássico: Dados de LiDAR geram nuvens de pontos ruidosas e esparsas. Classificadores lineares clássicos (como SVM padrão) falham quando a separação entre "grama" e "asfalto" não é clara devido ao ruído do sensor ou baixa resolução.

    Solução Quântica: Quantum Support Vector Machines (QSVM) com Kernel Quântico.

    Justificação (Baseada no PDF - Cap. 9): O livro dedica o Capítulo 9 às QSVMs. A vantagem reside no "truque do kernel": em vez de tentar separar os dados no espaço original ruidoso, mapeamos os dados para um espaço quântico de altíssima dimensão onde a separação se torna linear e clara.

    Mecanismo Técnico:

        Quantum Kernels: Projetam os dados do LiDAR (distância, refletividade) em um Espaço de Hilbert infinitamente dimensional.

        Vantagem: Onde um kernel clássico (RBF) vê apenas ruído sobreposto, o kernel quântico encontra hiperplanos de separação complexos, permitindo distinguir texturas de terreno com maior precisão.

A2 — Regressão de Deriva de IMU (Fácil)

   O Desafio Clássico

O viés (bias) do giroscópio não é uma constante perfeita; ele é obscurecido por um processo de Difusão (ruído estocástico e vibrações). Em janelas curtas de tempo, o desafio não é a integração temporal (trajetória), mas a separação de sinal-ruído: distinguir o que é uma rotação real ou um viés sistemático (Drift) do que é puramente agitação aleatória (Difusão). Métodos lineares clássicos frequentemente confundem alta difusão com mudanças de viés, resultando em calibrações errôneas.

Solução Quântica

Regressão de Kernel Quântico Baseada em Física (Physics-Informed Quantum Kernel Ridge Regression).

Justificativa

Diferente da abordagem recorrente (LSTM) focada em memória de longo prazo, esta solução foca na resolução instantânea. Como estamos analisando janelas curtas (W=32), a "memória" do movimento é menos relevante que a topologia dos dados. Utilizamos um Kernel Quântico para mapear os componentes de Drift e Difusão para um espaço de Hilbert de alta dimensão. Neste espaço, as interações não-lineares entre o ruído e o viés tornam-se linearmente separáveis, permitindo que um regressor Ridge isole a magnitude exata da deriva.

Mecanismo Técnico

    Pré-processamento Físico (Clássico):

        Decompomos o sinal bruto da IMU em dois vetores ortogonais de informação: o vetor de Drift (média, representando a tendência central bg​) e o escalar de Difusão (desvio padrão, representando a incerteza η).

    Mapeamento de Características (Quantum Feature Map):

        Codificamos esses termos físicos em estados quânticos usando um ZZFeatureMap.

        As portas de emaranhamento (ZZ) criam interferência entre os qubits que representam a Deriva e os que representam a Difusão. Isso modela fisicamente como o ruído "suja" o sinal do viés.

    Estimativa de Kernel (Quantum Kernel):

        O processador quântico calcula a fidelidade (similaridade) entre os estados. O Kernel atua como um filtro de similaridade não-linear: ele aprende a reconhecer padrões de viés (bg​) consistentes, ignorando a "nuvem" de difusão variável que os cerca. O resultado alimenta uma regressão Ridge clássica para a predição escalar final.

A3 — Detecção de Evento de Escorregamento (Médio)

    O Desafio Clássico: Detectar escorregamento (slip) exige fusão de sensores em tempo real. É um evento súbito e não linear onde a física do movimento muda drasticamente. Métodos clássicos baseados em limiares (thresholds) geram muitos falsos positivos em terrenos acidentados.

    Solução Quântica: Variational Quantum Classifier (VQC) [Capítulo 10].

    Justificação (Baseada no PDF - Cap. 10): O problema de escorregamento é, fundamentalmente, uma classificação binária complexa ("Escorregou" vs. "Aderente"). O Capítulo 10 detalha o uso de VQC (ou QNN Classifiers) para aprender fronteiras de decisão não lineares com poucos dados de treinamento (vantagem em robótica onde dados de falha são escassos).

    Mecanismo Técnico:

        PQC (Parametrized Quantum Circuit): Um circuito com portas de rotação ajustáveis (θ) e emaranhamento.

        Expressividade: A capacidade do circuito de gerar correlações quânticas permite detectar a assinatura "escondida" do escorregamento na correlação cruzada entre as rodas e a IMU, superando a sensibilidade de redes neurais clássicas pequenas.
2. Planejamento & Mapeamento

Foco: Otimização Combinatória (NP-difícil) e Grafos.

B1 — Micro‑TSP para Inspeção de Waypoints (Médio)

    O Desafio Clássico: O Caixeiro Viajante (TSP) é um problema combinatorial cuja complexidade cresce fatorialmente. Encontrar a rota mais curta que visita todos os pontos de inspeção e retorna à base é computacionalmente proibitivo para métodos exatos conforme o número de pontos aumenta.

    Solução Quântica: Quantum Approximate Optimization Algorithm (QAOA) [Capítulo 5] ou Grover Adaptive Search (GAS) [Capítulo 6].

    Justificação (Baseada no PDF - Cap. 3, 5 e 6): O livro utiliza explicitamente o TSP como exemplo clássico para modelagem QUBO (Quadratic Unconstrained Binary Optimization) no Capítulo 3. O Capítulo 5 demonstra como resolver estas formulações usando QAOA, e o Capítulo 6 oferece o GAS como uma alternativa de busca para minimizar custos em espaços de solução estruturados.

    Mecanismo Técnico:

        Formulação QUBO: O problema é traduzido numa matriz onde as variáveis binárias representam "visitar a cidade i no passo t".

        QAOA: Um circuito variacional aplica alternadamente operadores de "custo" (baseado na matriz QUBO) e "mistura" para amplificar a probabilidade de medir a rota ótima (estado de menor energia).

B3 — Seleção de Fechamentos de Loop via MWIS (Difícil)

    O Desafio Clássico: Em SLAM (Simultaneous Localization and Mapping), identificar quais "fechamentos de loop" (reconhecer um lugar já visitado) são válidos e quais são falsos positivos é crítico. Isso modela-se como o problema do Maximum Weight Independent Set (MWIS), que é NP-difícil. Um erro aqui corrompe todo o mapa.

    Solução Quântica: Otimização Variacional com Modelação Ising [Capítulo 3 & 5].

    Justificação (Baseada no PDF - Cap. 3): O MWIS é um problema de grafos que mapeia diretamente para o Modelo de Ising (spins magnéticos). O livro ensina no Capítulo 3 a converter problemas de grafos em Hamiltonianos físicos, onde a solução de menor energia corresponde à seleção ótima de loops consistentes.

    Mecanismo Técnico:

        Grafo de Conflitos: Cada possível fechamento de loop é um nó; arestas conectam loops que não podem ser verdadeiros simultaneamente.

        Ground State: O algoritmo (QAOA ou VQE) procura a configuração de spins (aceitar/rejeitar loop) que minimiza a energia do sistema, garantindo a consistência geométrica máxima do mapa.

3. Controle & Dinâmica

Foco: Otimização Contínua e Aprendizagem Híbrida.

C1 — IK (Cinemática Inversa) Sensível a Energia via Objetivo Variacional (Médio)

    O Desafio Clássico: Calcular os ângulos das juntas de um robô para atingir uma posição (x,y,z) minimizando o consumo de energia é um problema de otimização não linear e não convexo, frequentemente cheio de mínimos locais onde algoritmos clássicos (Gradiente Descendente) ficam presos.

    Solução Quântica: Variational Quantum Eigensolver (VQE) como Otimizador [Capítulo 7].

    Justificação (Baseada no PDF - Cap. 7): Embora o VQE seja famoso na química, o Capítulo 7 expande o seu uso para "problemas de otimização física geral". Podemos tratar a configuração do braço robótico como uma função de onda e a "energia mecânica + erro de posição" como o Hamiltoniano a ser minimizado.

    Mecanismo Técnico:

        Ansatz Variacional: Um circuito quântico parametrizado explora o espaço de configurações das juntas.

        Túnel Quântico (Efeito): A natureza quântica da otimização pode atravessar barreiras de potencial (mínimos locais) mais facilmente que métodos clássicos, encontrando configurações de braço mais eficientes energeticamente.

C3 — Atualização de Pose em Servo Visual (Difícil)

    O Desafio Clássico: Controlar um robô diretamente a partir de vídeo (pixels → torque) exige processar alta dimensionalidade em tempo real com latência mínima. Modelar a relação não linear entre a mudança na imagem e o movimento do motor é complexo.

    Solução Quântica: Hybrid Quantum Neural Networks (Regressão) [Capítulo 11].

    Justificação (Baseada no PDF - Cap. 11): Aqui aplica-se a estratégia híbrida "Transfer Learning" descrita no livro. Redes clássicas (CNNs) são insuperáveis no processamento inicial de imagens, mas a QNN brilha na tomada de decisão baseada nessas características.

    Mecanismo Técnico:

        Feature Extraction (Clássico): Uma ResNet pré-treinada reduz a imagem a um vetor compacto.

        Policy Network (Quântico): Uma QNN processa esse vetor num espaço de Hilbert para calcular a atualização de pose (Δx,Δy,Δθ). A alta expressividade da QNN (como visto na discussão de Difusão/QRC) permite ajustes de controle mais finos e robustos a ruído visual.

4. Segurança & Sistemas

Foco: Detecção de Anomalias e Generative AI.

D1 — Detecção de Anomalias em Telemetria (Médio)

    O Desafio Clássico: Identificar falhas (ciberataques ou defeitos mecânicos) em dados de telemetria complexos. Métodos estatísticos simples falham em capturar correlações subtis entre variáveis díspares (ex: temperatura da bateria vs. uso da CPU).

    Solução Quântica: Quantum Generative Adversarial Networks (QGAN) [Capítulo 12].

    Justificação (Baseada no PDF - Cap. 12): O Capítulo 12 é inteiramente dedicado a QGANs. Ao contrário de um classificador que precisa de exemplos de falhas (que são raros), a QGAN aprende a gerar dados "normais" perfeitos.

    Mecanismo Técnico:

        Aprendizagem da Distribuição: O Gerador Quântico aprende a distribuição de probabilidade da telemetria saudável.

        Detecção: Quando novos dados chegam, se o Discriminador (ou o próprio Gerador invertido) não conseguir reconhecê-los como parte da distribuição aprendida, é sinalizada uma anomalia. Isso é superior a métodos clássicos para detectar "black swan events" (eventos nunca antes vistos).
## 🧠 Módulos Principais
1.  `/quantum_perception`: Classificadores QSVM e QNN para visão computacional e LIDAR.
2.  `/quantum_navigation`: Implementações de QAOA para roteamento de múltiplos agentes.
3.  `/quantum_control`: Agentes de Reinforcement Learning Quântico (Q-RL) para controle motor.

---
