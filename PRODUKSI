import streamlit as st
from scipy.optimize import linprog
import matplotlib.pyplot as plt
import numpy as np

st.set_page_config(page_title="Optimasi Produksi", layout="centered")

st.title("📦 Aplikasi Optimasi Produksi - Linear Programming")
st.markdown("Masukkan data produksi dan sumber daya")

# Input
a_profit = st.number_input("Keuntungan per unit Produk A (Rp)", value=40000)
b_profit = st.number_input("Keuntungan per unit Produk B (Rp)", value=30000)

a_bahan = st.number_input("Kebutuhan bahan baku Produk A (kg)", value=2)
b_bahan = st.number_input("Kebutuhan bahan baku Produk B (kg)", value=1)
max_bahan = st.number_input("Total bahan baku tersedia (kg)", value=100)

a_jam = st.number_input("Jam kerja Produk A", value=3)
b_jam = st.number_input("Jam kerja Produk B", value=2)
max_jam = st.number_input("Total jam kerja tersedia", value=120)

if st.button("🔍 Optimalkan"):
    # Fungsi objektif (negatif untuk maximisasi)
    c = [-a_profit, -b_profit]
    
    # Matriks kendala
    A = [
        [a_bahan, b_bahan],
        [a_jam, b_jam]
    ]
    b_ub = [max_bahan, max_jam]
    
    x_bounds = (0, None)
    y_bounds = (0, None)

    result = linprog(c, A_ub=A, b_ub=b_ub, bounds=[x_bounds, y_bounds], method='highs')

    if result.success:
        x_opt, y_opt = result.x
        st.success(f"✔️ Kombinasi optimal:\n- Produk A: {x_opt:.2f} unit\n- Produk B: {y_opt:.2f} unit")
        st.info(f"💰 Total Keuntungan Maksimum: Rp {-(result.fun):,.0f}")
        
        # Visualisasi Area Feasible
        st.subheader("📈 Visualisasi Area Feasible")

        x = np.linspace(0, 80, 100)
        y1 = (max_bahan - a_bahan * x) / b_bahan
        y2 = (max_jam - a_jam * x) / b_jam

        plt.figure(figsize=(8,6))
        plt.plot(x, y1, label='Batas Bahan Baku')
        plt.plot(x, y2, label='Batas Jam Kerja')
        plt.fill_between(x, np.minimum(y1, y2), color='lightblue', alpha=0.5)

        plt.plot(x_opt, y_opt, 'ro', label='Solusi Optimal')
        plt.xlabel('Jumlah Produk A')
        plt.ylabel('Jumlah Produk B')
        plt.legend()
        plt.grid(True)
        st.pyplot(plt)
    else:
        st.error("❌ Solusi tidak ditemukan.")
