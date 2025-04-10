```
📄 Descripción general del proyecto
Nombre del código: Modelado Matemático para Estimación de Jugo de Naranja
Versión: 1.0
Explicación general: Este código implementa un modelo matemático simplificado para estimar la cantidad de jugo en una naranja, asumiendo una forma esférica. El modelo considera errores de modelado, medición y redondeo.
Qué problema resuelve el código: Estima la cantidad de jugo en una naranja de forma no invasiva, utilizando mediciones simples y un modelo matemático.

⚙️ Visión general del sistema
Arquitectura del sistema:
```mermaid
graph LR
A[Mediciones: Diámetro] --> B(Modelo Matemático: Esfera)
B --> C{Cálculo: Volumen}
C --> D[Relación: q = α * v]
D --> E(Estimación: Cantidad de Jugo)
```
Tecnologías utilizadas:
Julia, Pluto.jl, PlutoUI, Colors, ColorVectorSpace, ImageShow, FileIO, ImageIO, Plots, HypertextLiteral
Dependencias:
- PlutoUI
- Colors
- ColorVectorSpace
- ImageShow
- FileIO
- ImageIO
- Plots
- HypertextLiteral
Requisitos del sistema:
- Julia 1.0 o superior
- Pluto.jl
Prerrequisitos:
- Tener Julia instalado.
- Tener Pluto.jl instalado.
- Tener las dependencias instaladas.

📦 Guía de uso
Cómo usarlo:
El cuaderno de Pluto.jl proporciona una interfaz interactiva para explorar el modelo matemático.
Explicación de los pasos:
1.  Entrada: El usuario interactúa con un slider para definir el radio de la esfera (ρ).
2.  Procesamiento: El código calcula el volumen de la esfera usando la fórmula v = (4/3) * π * ρ³.
3.  Salida: Se genera una visualización de la esfera y se presenta una estimación de la cantidad de jugo basada en el volumen y un factor de proporción α.
Caso de uso de ejemplo:
```julia
using Plots
ρ = 5 # Radio de la esfera
alpha = 0.7 # Factor de proporción (cantidad de jugo / volumen)
v = (4/3) * pi * ρ^3 # Volumen de la esfera
q = alpha * v # Estimación de la cantidad de jugo
println("Estimación de la cantidad de jugo: ", q)
# Graficar la esfera
theta_range = range(0, 2π, length=100)
phi_range = range(0, π, length=100)

# Generar los puntos de la esfera
x = [ρ * sin(φ) * cos(θ) for φ in phi_range, θ in theta_range]
y = [ρ * sin(φ) * sin(θ) for φ in phi_range, θ in theta_range]
z = [ρ * cos(φ) for φ in phi_range, θ in theta_range]

# Graficar la esfera
surface(x, y, z, xlabel="X", ylabel="Y", zlabel="Z", title="Esfera de Radio p = $ρ")
```

📚 Referencias
1.  Lin, C. C., & Segel, L. A. (1988). Mathematics Applied to Deterministic Problems in the Natural Sciences (Classics in Applied Mathematics, Series Number 1) (1st ed.). SIAM: Society for Industrial and Applied.
2.  Kincaid, D. R., & Cheney, E. W. (1996). Numerical Analysis: Mathematics of Scientific Computing (2nd ed.). Brooks Cole.
3.  Strang, G. (1986). Introduction to Applied Mathematics. Wellesley-Cambridge Press
```
