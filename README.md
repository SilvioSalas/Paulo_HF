Foi construído e depurado um pipeline completo de Análise de Sentimentos em Português rodando na GPU T4 gratuita do Google Colab:
• ModernBERT (answerdotai/ModernBERT-base): Testamos a arquitetura moderna, descobrimos na prática que o Hugging Face deixa o backbone descongelado por padrão e aplicamos o congelamento manual 
(requires_grad = False). Identificamos também o fenômeno de overfitting e a barreira do idioma (já que o backbone congelado foi treinado em inglês).
• BERTimbau (neuralmind/bert-base-portuguese-cased): Migramos para um modelo nativo em português, validamos a troca de cabeça (29.794 saídas de vocabulário substituídas por 2 saídas de classificação) e 
confirmamos o congelamento extremo: apenas 1.538 parâmetros treináveis (0.0014%) de um total de ~109 milhões.
• Ajuste de Hiperparâmetros: Ajustamos a taxa de aprendizado (learning_rate=1e-2) e os passos para compensar o fato de estarmos treinando apenas uma única camada linear sobre um backbone 100% congelado, fazendo a training loss cair de 0.69 para a faixa de 0.22.
