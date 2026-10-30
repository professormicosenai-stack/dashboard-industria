import streamlit as st
import pandas as pd
import altair as alt

# Configuração da página e layout amplo
st.set_page_config(page_title="Dashboard SINE - Norte Pioneiro", layout="wide")

# Estilização CSS: Centralização perfeita, espaçamento horizontal enxuto e altura vertical compacta
st.markdown("""
    <style>
    .main {
        background-color: #f4f6f9;
    }
    .block-container {
        padding-top: 1.2rem !important;
        padding-bottom: 1rem !important;
        max-width: 82rem !important;
        margin: auto;
    }
    .header-container {
        background: linear-gradient(90deg, #003366 0%, #004B87 100%);
        padding: 15px 20px;
        border-radius: 10px;
        color: white;
        margin-bottom: 12px;
        box-shadow: 0 3px 5px rgba(0,0,0,0.1);
        text-align: center;
    }
    .header-title {
        font-size: 24px;
        font-weight: 700;
        margin: 0;
    }
    .header-subtitle {
        font-size: 14px;
        opacity: 0.95;
        margin-top: 3px;
    }
    .kpi-card {
        background: linear-gradient(135deg, #004B87 0%, #005A9C 100%);
        color: white;
        padding: 12px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.08);
        text-align: center;
    }
    .kpi-title {
        font-size: 12px;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.5px;
        margin: 0;
        opacity: 0.9;
    }
    .kpi-value {
        font-size: 20px;
        font-weight: 700;
        margin: 4px 0 0 0;
    }
    .content-box {
        background-color: #ffffff;
        border-top: 4px solid #004B87;
        padding: 15px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        margin-bottom: 10px;
        text-align: center;
    }
    </style>
""", unsafe_allow_html=True)

# Topo do Painel
st.markdown("""
    <div class="header-container">
        <div class="header-title">SINE | Sistema Nacional de Emprego — gov.br</div>
        <div class="header-subtitle">Painel Estratégico de Empregabilidade - Norte Pioneiro (PR) — Foco na Indústria</div>
    </div>
""", unsafe_allow_html=True)

# Dados da Região Norte Pioneiro
kpi_vagas_industria = 1420
kpi_salario_medio_regiao = 2150.00
kpi_media_idade = 31.5
pct_tecnicos = "42.3%"

# ==================== KPIS DE DESTAQUE ====================
col1, col2, col3, col4 = st.columns(4)

with col1:
    st.markdown(f"""
        <div class="kpi-card">
            <p class="kpi-title">Vagas na Indústria (SINE)</p>
            <p class="kpi-value">{kpi_vagas_industria} vagas</p>
        </div>
    """, unsafe_allow_html=True)

with col2:
    st.markdown(f"""
        <div class="kpi-card">
            <p class="kpi-title">Média Salarial (Região)</p>
            <p class="kpi-value">R$ {kpi_salario_medio_regiao:,.2f}</p>
        </div>
    """, unsafe_allow_html=True)

with col3:
    st.markdown(f"""
        <div class="kpi-card">
            <p class="kpi-title">Média de Idade</p>
            <p class="kpi-value">{kpi_media_idade} anos</p>
        </div>
    """, unsafe_allow_html=True)

with col4:
    st.markdown(f"""
        <div class="kpi-card">
            <p class="kpi-title">Candidatos c/ Técnico</p>
            <p class="kpi-value">{pct_tecnicos}</p>
        </div>
    """, unsafe_allow_html=True)

# ==================== SEÇÃO PRINCIPAL (LADO A LADO) ====================
col_a, col_b = st.columns(2)

with col_a:
    st.markdown("""
        <div class="content-box">
            <h3 style="color: #003366; font-size: 16px; margin-top:0; margin-bottom: 3px;">Média Salarial por Qualificação</h3>
            <p style="font-size: 13px; color: #555; margin-bottom: 8px;">Comparativo direto de remuneração nas oportunidades industriais.</p>
        </div>
    """, unsafe_allow_html=True)
    
    salario_df = pd.DataFrame({
        "Nível": ["Sem Curso Técnico", "Com Curso Técnico"],
        "Salario": [1850.00, 2780.00],
        "Texto_Valor": ["R$ 1.850,00", "R$ 2.780,00"]
    })
    
    chart = alt.Chart(salario_df).mark_bar(width=45, color="#004B87", cornerRadiusTopLeft=5, cornerRadiusTopRight=5).encode(
        x=alt.X('Nível', sort=None, title='', axis=alt.Axis(labelAngle=0, labelFontSize=11)),
        y=alt.Y('Salario', title='Média Salarial (R$)', scale=alt.Scale(domain=[0, 3300]), axis=alt.Axis(labelFontSize=11))
    ).properties(height=210)
    
    text = chart.mark_text(
        align='center',
        baseline='bottom',
        dy=-6,
        fontSize=12,
        fontWeight='bold',
        color='#003366'
    ).encode(
        text='Texto_Valor:N'
    )
    
    st.altair_chart(chart + text, use_container_width=True)

with col_b:
    st.markdown("""
        <div class="content-box" style="padding-bottom: 10px;">
            <h3 style="color: #003366; font-size: 16px; margin-top:0; margin-bottom: 3px;">Perfil Educacional Médio</h3>
            <p style="font-size: 13px; color: #555; margin-bottom: 10px;">Distribuição qualificada da base de trabalhadores cadastrados.</p>
        </div>
    """, unsafe_allow_html=True)
    
    st.info("**Escolaridade Predominante (Ensino Médio Completo):** 68.5% dos cadastrados")
    st.success("**Taxa de Formados em Cursos Técnicos:** 42.3% dos cadastrados")
    
    st.markdown("""
        <div style="background-color: #ffffff; padding: 10px; border-radius: 6px; border-top: 3px solid #004B87; box-shadow: 0 1px 3px rgba(0,0,0,0.04); text-align: center; margin-top: 5px;">
            <p style='font-size: 12px; color: #555; margin: 0; line-height: 1.3;'>
                Evidência clara do potencial de sinergia entre as demandas do SINE e a qualificação profissional técnica no Norte Pioneiro.
            </p>
        </div>
    """, unsafe_allow_html=True)

# ==================== TOP 10 CURSOS ====================
st.markdown("""
    <div class="content-box" style="padding: 12px; margin-bottom: 8px; margin-top: 5px;">
        <h3 style="color: #003366; font-size: 16px; margin-top:0; margin-bottom: 3px;">Top 10 Cursos Informados pelos Candidatos (SINE)</h3>
        <p style="font-size: 13px; color: #555; margin: 0;">Principais qualificações técnicas presentes no currículo dos trabalhadores.</p>
    </div>
""", unsafe_allow_html=True)

top_cursos = pd.DataFrame({
    "Curso / Capacitação": [
        "Eletromecânica / Eletricidade Industrial",
        "Mecânica de Usinagem",
        "Segurança do Trabalho (NRs)",
        "Logística e Operação de Empilhadeira",
        "Informática / Pacote Office",
        "Soldagem Básica",
        "Gestão de Produção",
        "Manutenção Automotiva",
        "Alimentos e Boas Práticas",
        "Administração de Empresas"
    ],
    "Candidatos Cadastrados": [340, 290, 275, 250, 210, 180, 150, 130, 115, 95]
})

_, col_tabela, _ = st.columns([0.1, 0.8, 0.1])
with col_tabela:
    st.dataframe(top_cursos, use_container_width=True, hide_index=True, height=385)
