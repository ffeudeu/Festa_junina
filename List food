import streamlit as st
import json
import os

# Nome do arquivo onde os dados ficarão salvos
ARQUIVO_DADOS = 'dados_festa.json'

# Listas iniciais de pratos
DOCES = ["Canjica", "Pé de Moleque", "Bolo de Milho", "Bolo de Fubá", "Paçoca", "Arroz Doce", "Maçã do Amor", "Cocada", "Curau", "Doce de Abóbora"]
SALGADOS = ["Cachorro Quente", "Pipoca", "Caldo Verde", "Milho Cozido", "Pamonha", "Pastel", "Cuscuz", "Torta Salgada", "Espetinho", "Pão de Queijo"]

# Função para carregar os dados salvos ou criar um novo se não existir
def carregar_dados():
    if os.path.exists(ARQUIVO_DADOS):
        with open(ARQUIVO_DADOS, 'r', encoding='utf-8') as f:
            return json.load(f)
    return {"doces_disponiveis": DOCES, "salgados_disponiveis": SALGADOS, "convidados": []}

# Função para salvar os dados atualizados
def salvar_dados(dados):
    with open(ARQUIVO_DADOS, 'w', encoding='utf-8') as f:
        json.dump(dados, f, ensure_ascii=False, indent=4)

# Inicializa o app e carrega os dados
dados = carregar_dados()

st.title("🔥 Lista da Festa Junina 🔥")
st.write("Confirme sua presença e escolha o que vai trazer para o nosso arraiá!")

# Formulário do Convidado
nome = st.text_input("Seu Nome:")
acompanhantes = st.number_input("Quantas pessoas você vai levar com você?", min_value=0, max_value=15, step=1)

st.divider()

st.subheader("🥘 Escolha dos Pratos")
st.info("Regra: Se você levar **2 ou mais convidados**, é obrigatório escolher pelo menos uma opção de Doce e uma de Salgado.")

# Opções de múltipla escolha (Selectbox) para os pratos que ainda estão disponíveis
doce_escolhido = st.selectbox("Escolha um prato DOCE", ["Nenhum"] + dados["doces_disponiveis"])
salgado_escolhido = st.selectbox("Escolha um prato SALGADO", ["Nenhum"] + dados["salgados_disponiveis"])

if st.button("Confirmar Presença"):
    if not nome.strip():
        st.error("Por favor, preencha o seu nome!")
    else:
        trouxe_doce = doce_escolhido != "Nenhum"
        trouxe_salgado = salgado_escolhido != "Nenhum"
        
        # Validação da regra de convidados
        if acompanhantes >= 2 and not (trouxe_doce and trouxe_salgado):
            st.error("Como você vai levar 2 ou mais pessoas, por favor selecione um prato Doce E um prato Salgado.")
        elif not trouxe_doce and not trouxe_salgado:
            st.error("Por favor, selecione pelo menos um prato (doce ou salgado) para trazer!")
        else:
            # Remove as opções escolhidas para que os próximos não vejam
            if trouxe_doce:
                dados["doces_disponiveis"].remove(doce_escolhido)
            if trouxe_salgado:
                dados["salgados_disponiveis"].remove(salgado_escolhido)
            
            # Adiciona o convidado na lista
            dados["convidados"].append({
                "nome": nome.strip(),
                "acompanhantes": acompanhantes,
                "doce": doce_escolhido if trouxe_doce else "---",
                "salgado": salgado_escolhido if trouxe_salgado else "---"
            })
            
            salvar_dados(dados)
            st.success(f"Presença confirmada, {nome}! Os pratos foram reservados.")
            st.rerun() # Recarrega a página para atualizar as listas

st.divider()

# Exibição de quem já confirmou
st.subheader("📋 Lista de Confirmados")
if dados["convidados"]:
    for c in dados["convidados"]:
        total_pessoas = c['acompanhantes'] + 1
        st.write(f"🤠 **{c['nome']}** (Total: {total_pessoas} pessoas)")
        st.write(f"↳ Vai trazer: 🍬 {c['doce']} | 🌭 {c['salgado']}")
else:
    st.write("Ninguém confirmou ainda. Seja o primeiro!")
