# Investigação: CloutHaus — Social Media Leads to Compromise

## 1. Contexto e Objetivo
Investigação de um incidente de segurança envolvendo o comprometimento das contas da influenciadora **Afomiya Storm** (ambiente simulado CloutHaus). O objetivo da análise foi reconstruir a linha do tempo do ataque, identificar o vetor inicial de acesso e mapear o impacto da exposição de dados após o comprometimento das credenciais.

## 2. Hipóteses e Metodologia de Busca
A análise partiu da hipótese de comprometimento de conta via engenharia social e coleta prévia de informações públicas (OSINT). A investigação foi conduzida por meio de consultas KQL correlacionando dados cadastrais, registros de comunicação e telemetria de rede:

* **Tabelas analisadas:** `Employees`, `Email`, `OutboundNetworkEvents`, `InboundNetworkEvents` e `PassiveDns`.
* **Operadores KQL aplicados:** Uso de filtros direcionados (`where`) para isolar eventos relacionados à vítima e deduplicação de registros (`distinct`) para mapear domínios, remetentes e conexões de saída suspeitas.

## 3. Artefatos e Evidências Identificadas
* **Reconhecimento (OSINT e Engenharia Social):**
  * **Busca Reversa de Imagem (OSINT):** Identificação do endereço residencial da vítima a partir de uma foto pública de seu apartamento.
  * **Coleta de Perguntas de Segurança:** Exploração de caixas de perguntas nos *stories* do Instagram para obter respostas de recuperação de conta (nome da escola do ensino médio, primeiro animal de estimação e nome de solteira da mãe).
* **Vetor Inicial (Spear Phishing):** Envio de e-mail fraudulento simulando uma parceria exclusiva com a marca Dior, contendo link malicioso para captura de credenciais (*credential harvesting*).
* **Movimentação e Impacto:**
  * Devido à **reutilização de senhas** entre plataformas e ao comprometimento das perguntas de segurança, os atacantes obtiveram acesso simultâneo ao **e-mail** e ao **Instagram** da influenciadora.
  * **Abuso de Conta (Instagram):** Utilização do perfil comprometido para aplicar fraudes financeiras contra seguidores sob o pretexto de uma falsa "oportunidade de investimento".
  * **Exfiltração de Dados (E-mail):** Acesso não autorizado à caixa de entrada e **encaminhamento de mensagens** contendo documentos sensíveis (dados de passaporte, registros de pagamentos e informações pessoais).

## 4. Remediação e Lições Aprendidas
* **Contenção:** O acesso indevido às contas foi encerrado mediante a redefinição imediata das credenciais comprometidas e a ativação da **Autenticação Multifator (MFA)**.
* **Lições Técnicas:**
  * A exposição excessiva de informações em redes sociais reduz o custo de reconhecimento para ataques direcionados (OSINT).
  * A reutilização de senhas transforma um único incidente de phishing em um comprometimento em cascata; o uso de gerenciadores de senhas e MFA são controles críticos para mitigar o sequestro de contas (*Account Takeover*).
  * A correlação entre logs de e-mail (`Email`) e eventos de rede (`OutboundNetworkEvents` / `PassiveDns`) permite validar se houve clique efetivo em links de phishing dentro do ambiente monitorado.
