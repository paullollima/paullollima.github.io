---
layout: default
title: Agendamento LabP2D
---



<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Agendamento de Recursos</title>
</head>
<body>
  <h1>Agende um Recurso</h1>
  <form id="agendamentoForm">
    <label for="nome">Seu Nome:</label><br>
    <input type="text" id="nome" required><br><br>

    <label for="titulo">Recurso a Agendar:</label><br>
    <input type="text" id="titulo" required><br><br>

    <label for="inicio">Data e Hora de Início:</label><br>
    <input type="datetime-local" id="inicio" required><br><br>

    <label for="fim">Data e Hora de Término:</label><br>
    <input type="datetime-local" id="fim" required><br><br>

    <button type="submit">Agendar</button>
  </form>

  <p id="mensagem"></p>

  <script>
    const form = document.getElementById('agendamentoForm');
    const mensagem = document.getElementById('mensagem');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();

      const dados = {
        titulo: document.getElementById('titulo').value,
        inicio: document.getElementById('inicio').value,
        fim: document.getElementById('fim').value,
        descricao: "Agendado por " + document.getElementById('nome').value
      };

      try {
        const resposta = await fetch('https://script.google.com/macros/s/AKfycbwHVXcy8HB4TXJMlEO49zUsTTszJnrsg2hvxVbxvWM9-sx5qe4WU9GVEV4EJHqly2lE/exec', {
          method: 'POST',
          body: JSON.stringify(dados),
          headers: { 'Content-Type': 'application/json' }
        });

        const resultado = await resposta.json();

        if (resultado.status === 'sucesso') {
          mensagem.innerText = "✅ Evento agendado com sucesso!";
          form.reset();
        } else {
          mensagem.innerText = "⚠️ Erro: " + resultado.mensagem;
        }

      } catch (erro) {
        mensagem.innerText = "❌ Erro ao conectar com o servidor.";
        console.error(erro);
      }
    });
  </script>
</body>
</html>


<!-- 
<div class="form-container">
  <h2>Agende uma Reunião com o LabP2D</h2>
  
  <form id="agendamentoForm">
    <!-- Campo Nome ->
    <div class="form-group">
      <label for="nome">Nome Completo*</label>
      <input type="text" id="nome" name="nome" required>
    </div>
    
    <!-- Campo Email ->
    <div class="form-group">
      <label for="email">Email Institucional*</label>
      <input type="email" id="email" name="email" required>
    </div>
    
    <!-- Campo Data/Horário ->
    <div class="form-group">
      <label for="data">Data e Horário*</label>
      <input type="datetime-local" id="data" name="data" required>
    </div>
    
    <!-- Campo Tipo de Reunião ->
    <div class="form-group">
      <label for="tipo">Tipo de Reunião*</label>
      <select id="tipo" name="tipo" required>
        <option value="" disabled selected>Selecione uma opção</option>
        <option value="pesquisa">Colaboração em Pesquisa</option>
        <option value="projeto">Discussão de Projeto</option>
        <option value="orientacao">Orientação Acadêmica</option>
        <option value="outro">Outro Assunto</option>
      </select>
    </div>
    
    <!-- Campo Mensagem ->
    <div class="form-group">
      <label for="mensagem">Detalhes Adicionais</label>
      <textarea id="mensagem" name="mensagem" rows="4" placeholder="Descreva brevemente o propósito da reunião"></textarea>
    </div>
    
    <!-- Botão de Envio ->
    <button type="submit" class="btn-agendar">
      <span class="submit-text">Solicitar Agendamento</span>
      <span class="loading-icon" style="display:none;">⌛ Enviando...</span>
    </button>
    
    <!-- Mensagens de Status ->
    <div id="formStatus" class="form-status" style="display:none;"></div>
  </form>
</div> -->

<!-- Link para voltar ->
<div style="text-align: center; margin-top: 30px;">
  <a href="/" class="btn-voltar"> Voltar para a página principal</a>
</div>

<style>
/* Estilos específicos para a página de agendamento */
.form-container {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
}

h2 {
  color: #007ff5;
  text-align: center;
  margin-bottom: 30px;
}

.btn-voltar {
  color: #4276b6;
  padding: 8px 16px;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.btn-voltar:hover {
  background: #f5f5f5;
  text-decoration: underline;
}
</style>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const form = document.getElementById('agendamentoForm');
  const statusElement = document.getElementById('formStatus');
  const submitBtn = form.querySelector('button[type="submit"]');
  const submitText = submitBtn.querySelector('.submit-text');
  const loadingIcon = submitBtn.querySelector('.loading-icon');

  form.addEventListener('submit', async function(e) {
    e.preventDefault();
    
    // Mostrar estado de carregamento
    submitText.style.display = 'none';
    loadingIcon.style.display = 'inline';
    submitBtn.disabled = true;
    
    // Preparar dados do formulário
    const formData = {
      nome: document.getElementById('nome').value,
      email: document.getElementById('email').value,
      data: document.getElementById('data').value,
      tipo: document.getElementById('tipo').value,
      mensagem: document.getElementById('mensagem').value
    };
    
    try {
      // Substitua pela URL do seu Google Apps Script
      const response = await fetch('https://script.google.com/macros/s/SUA_URL_DO_SCRIPT/exec', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData)
      });
      
      const result = await response.json();
      
      if (result.status === 'success') {
        showStatus('Agendamento solicitado com sucesso! Você receberá um email de confirmação em breve.', 'success');
        form.reset();
      } else {
        throw new Error(result.message || 'Erro no servidor');
      }
    } catch (error) {
      showStatus(`Erro ao agendar: ${error.message}`, 'error');
      console.error('Erro:', error);
    } finally {
      // Restaurar estado normal do botão
      submitText.style.display = 'inline';
      loadingIcon.style.display = 'none';
      submitBtn.disabled = false;
    }
  });
  
  function showStatus(message, type) {
    statusElement.textContent = message;
    statusElement.className = `form-status ${type}`;
    statusElement.style.display = 'block';
    
    // Esconder após 5 segundos
    setTimeout(() => {
      statusElement.style.display = 'none';
    }, 5000);
  }
});
</script> -->

