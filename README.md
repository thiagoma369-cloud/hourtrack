# 📊 Relatório de Serviços de Máquinas Pesadas

Um sistema web desenvolvido em **Vue 3 (Options API)** para **controle completo de serviços de máquinas pesadas**, como tratores, carregadeiras, rolos e empilhadeiras. Permite registrar serviços por hora, controlar despesas, calcular lucro líquido e gerar relatórios financeiros de forma intuitiva.

---

## 🚀 Funcionalidades Principais

- **Cadastro de serviços**
  - Data do serviço
  - Máquina utilizada
  - Contratante
  - Tipo de serviço (Arado, Roço, Plantio, etc.)
  - Valor por hora e quantidade de horas
  - Desconto opcional
  - Adição de despesas detalhadas por serviço

- **Gestão de despesas**
  - Adicionar múltiplas despesas
  - Remover despesas individuais
  - Total de despesas do mês calculado automaticamente

- **Resumo financeiro**
  - Total bruto
  - Total de descontos
  - Total de despesas
  - Total líquido
  - Média de valor por hora
  - Serviço mais lucrativo

- **Filtros inteligentes**
  - Filtrar serviços por tipo
  - Filtrar serviços por contratante
  - Ordenação automática por data (mais recente primeiro)

- **Persistência de dados**
  - Todos os serviços e despesas são salvos no **LocalStorage**, mantendo os dados ao fechar o navegador

---

## 🏗 Estrutura do Projeto


/src
├─ App.vue # Componente principal
├─ FormularioServico.vue # Formulário de registro de serviços
├─ ListaServicos.vue # Lista de serviços cadastrados
├─ FiltrosServicos.vue # Filtros para visualização
├─ ResumoFinanceiro.vue # Resumo financeiro com métricas
└─ Despesas.vue # Componente de gerenciamento de despesas


---

## 💻 Como Executar o Projeto

1. Instalar dependências:

npm install

Rodar a aplicação em modo desenvolvimento:

npm run dev

Abrir no navegador:

http://localhost:4040

Todos os dados inseridos ficam salvos localmente no navegador.

🎨 Tecnologias Utilizadas

Vue 3 – Framework JavaScript para aplicações reativas

HTML5 & CSS3 – Estrutura e estilização

JavaScript ES6 – Lógica de cálculos, filtros e manipulação de arrays

LocalStorage – Persistência local de dados

📈 Próximos Recursos Planejados

Exportar relatórios em PDF ou Excel

Gráficos financeiros por mês

Sistema de login para múltiplos usuários

Aplicativo mobile via Capacitor ou Vue Native

⚖ Licença

Projeto sob licença MIT. Consulte o arquivo LICENSE para mais detalhes.

👤 Autor

Thiago Matheus da Silva Santos
Desenvolvedor e idealizador do projeto, focado em soluções práticas de gestão financeira para serviços de máquinas pesadas.