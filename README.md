import streamlit as st
import matplotlib.pyplot as plt
import matplotlib.patches as patches
from matplotlib.animation import FuncAnimation
from io import BytesIO
import base64

st.set_page_config(page_title="Simulasi Uji Molisch", layout="centered")

st.title("🧪 Simulasi Uji Molisch untuk Karbohidrat")
st.markdown("""
Uji Molisch adalah uji **kualitatif untuk mendeteksi karbohidrat**. Hasil positif ditunjukkan dengan **cincin ungu** di batas dua cairan.
""")

start_test = st.button("🔬 Jalankan Uji Molisch")

if start_test:
    fig, ax = plt.subplots(figsize=(4, 8))
    plt.axis('off')
    ax.set_xlim(0, 4)
    ax.set_ylim(0, 8)

    # Tabung reaksi
    tabung = patches.FancyBboxPatch((1, 1), 2, 6,
                                    boxstyle="round,pad=0.02",
                                    edgecolor='black', facecolor='white', linewidth=2)
    ax.add_patch(tabung)

    # Larutan karbohidrat
    karbohidrat = patches.Rectangle((1.1, 1.2), 1.8, 3, facecolor='#add8e6')
    ax.add_patch(karbohidrat)

    # Tetesan pereaksi Molisch
    molisch_tetes = patches.Circle((2, 6.5), 0.1, color='purple', alpha=0.0)
    ax.add_patch(molisch_tetes)

    # Cincin ungu
    cincin_ungu = patches.Rectangle((1.1, 4.2), 1.8, 0.1, facecolor='purple', alpha=0.0)
    ax.add_patch(cincin_ungu)

    def animate(frame):
        if frame < 10:
            molisch_tetes.set_alpha(frame / 10)
            molisch_tetes.set_center((2, 6.5 - frame * 0.3))
        elif frame < 20:
            molisch_tetes.set_alpha(0.0)
            cincin_ungu.set_alpha((frame - 10) / 10)

    ani = FuncAnimation(fig, animate, frames=30, interval=100)

    # Simpan animasi sebagai GIF
    buf = BytesIO()
    ani.save(buf, format='gif', writer='pillow')
    buf.seek(0)
    gif_base64 = base64.b64encode(buf.read()).decode("utf-8")
    st.markdown(f'<img src="data:image/gif;base64,{gif_base64}" width="300">', unsafe_allow_html=True)

    # Interpretasi
    st.subheader("🟣 Interpretasi Hasil")
    st.success("✅ Terbentuk cincin ungu → Hasil positif\n\nSampel mengandung **karbohidrat**.")
    st.markdown("""
    **Penjelasan Reaksi:**
    - Karbohidrat diubah menjadi furfural oleh H₂SO₄ pekat.
    - Furfural be
