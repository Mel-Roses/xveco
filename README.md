# xveconomy

Librería avanzada de economía, bancos, mercados e inventarios en Python, con soporte de peso, volumen, durabilidad, podrición y contenedores.

**Versión:** 2.1.5  
**Licencia:** AGPLv3 o posterior  
**Python:** >=3.8  

## Instalación

pip install xveconomy

## Características

- Monedas con conversión a EUR e inflación
- Bancos con préstamos, deudas e intereses
- Cuentas con saldo, inventario, peso y volumen máximos
- Ítems con durabilidad y podrición basada en tiempo
- Contenedores (mochilas) con límites propios de peso y huecos
- Mercado con precios dinámicos según oferta/demanda
- Sistema de intercambios (trades) entre cuentas

## Uso básico

### Moneda y cuenta

from xveco import Currency, Account

euro = Currency("Euro", "€", 1.0, logo="💶")
cuenta = Account("Jugador1", euro)

cuenta.deposit(100)
cuenta.show_balance()

### Banco

from xveco import Bank

banco = Bank(euro, supply=10000)

banco.loan(cuenta, 500, interest=5)
print(banco.calculate_debt(cuenta))

banco.pay_debt(cuenta)

### Ítems e inventario

from xveco import Item, ItemInstance

manzana = Item("manzana", "Manzana", base_price=2, weight=0.2, volume=0.1, rots=True, decay_time=60)
instancia = ItemInstance(manzana)

cuenta.add_item_instance(instancia)
print(instancia.get_current_decay())

### Contenedores

from xveco import Container

mochila = Container("mochila1", "Mochila básica", max_container_slots=10, max_weight=20)
mochila.add_item(instancia)

### Mercado

from xveco import Market

mercado = Market(euro)
mercado.register_item(manzana)

mercado.buy(cuenta, "manzana", qty=3)
mercado.sell(cuenta, "manzana", qty=1)

### Intercambios (Trade)

from xveco import Trade

cuenta2 = Account("Jugador2", euro)
trade = Trade(cuenta, cuenta2)

trade.add_offer(cuenta, items=[], money=50)
trade.add_offer(cuenta2, items=[instancia], money=0)

trade.accept(cuenta)
trade.accept(cuenta2)

## Clases principales

| Clase | Descripción |
|---|---|
| `Currency` | Moneda con valor a EUR e inflación |
| `Bank` | Préstamos, deudas y suministro de dinero |
| `Item` | Definición de un ítem (plantilla) |
| `ItemInstance` | Instancia concreta de un ítem, con durabilidad/podrición |
| `Container` | Contenedor con límites de peso y huecos |
| `Account` | Cuenta de usuario con saldo e inventario |
| `Market` | Mercado con precios dinámicos |
| `Trade` | Intercambio entre dos cuentas |

---

Librería de economía simulada para Python.

xveco permite crear monedas, cuentas bancarias, sistemas de inflación, bancos y préstamos de forma sencilla. Está pensada para videojuegos, bots, simulaciones económicas y proyectos educativos.

---

## 💰 Características

- Monedas personalizadas
- Cuentas con saldo en céntimos
- Depósitos y retiradas
- Transferencias entre usuarios
- Conversión a euros
- Sistema de inflación
- Bancos con dinero limitado
- Préstamos con interés
- Gestión de deudas
- API simple y fácil de usar

---

## 📦 Instalación

pip install xveconomy

---

## 🚀 Ejemplo completo

import xveco

# Crear moneda
coin = xveco.Currency(
    "XCoin",
    "X",
    0.5,
    "🪙"
)

# Crear cuentas
santi = xveco.Account("Santi", coin)
pepe = xveco.Account("Pepe", coin)

# Ingresar dinero
santi.deposit(100)
pepe.deposit(50)

# Transferir dinero
santi.transfer(pepe, 25)

# Mostrar saldos
santi.show_balance()
pepe.show_balance()

# Aplicar inflación
coin.apply_inflation(10)

print("Saldo de Santi en euros:", santi.balance_in_eur())

Salida aproximada:

🪙 Santi: 75,00 X
🪙 Pepe: 75,00 X

Saldo de Santi en euros: 41.25

---

# 📚 Documentación

## 🪙 Currency

Representa una moneda.

### Crear una moneda

coin = xveco.Currency(
    "XCoin",
    "X",
    0.5,
    "🪙"
)

### Parámetros

| Parámetro    | Tipo       | Descripción            |
| ------------ | ---------- | ---------------------- |
| name         | str        | Nombre de la moneda    |
| symbol       | str        | Símbolo de la moneda   |
| value_to_eur | float      | Valor respecto al euro |
| logo         | str / None | Emoji o texto opcional |

### Aplicar inflación

coin.apply_inflation(10)

Incrementa el valor de la moneda un 10%.

---

## 👤 Account

Representa una cuenta bancaria.

### Crear una cuenta

user = xveco.Account("Usuario", coin)

### Ingresar dinero

user.deposit(100)

### Retirar dinero

user.withdraw(50)

### Transferir dinero

user1.transfer(user2, 25)

### Mostrar saldo

user.show_balance()

Ejemplo:

🪙 Usuario: 125,00 X

### Obtener saldo en euros

print(user.balance_in_eur())

---

## 🏦 Bank

Representa un banco con una cantidad limitada de dinero.

### Crear banco

bank = xveco.Bank(
    coin,
    10000
)

---

### Conceder préstamo

bank.loan(
    santi,
    100,
    10
)

Parámetros:

| Parámetro | Descripción                   |
| --------- | ----------------------------- |
| account   | Cuenta que recibe el préstamo |
| amount    | Cantidad prestada             |
| interest  | Interés (%)                   |

---

### Calcular deuda

debt = bank.calculate_debt(santi)

print(debt)

---

### Pagar deuda

bank.pay_debt(santi)

---

## 💡 Ejemplo de préstamo

import xveco

coin = xveco.Currency(
    "XCoin",
    "X",
    1,
    "🪙"
)

bank = xveco.Bank(
    coin,
    1000
)

santi = xveco.Account(
    "Santi",
    coin
)

bank.loan(
    santi,
    100,
    10
)

print(
    "Deuda:",
    bank.calculate_debt(santi)
)

santi.deposit(20)

bank.pay_debt(santi)

santi.show_balance()

---

## ⚙️ Cómo funciona internamente

Los saldos se almacenan en céntimos para evitar errores de precisión causados por números decimales.

Ejemplo:

user.deposit(1.50)

Internamente:

150

---

## 🎯 Casos de uso

- Bots de Discord
- Videojuegos
- RPGs
- Sistemas de comercio
- Simulaciones económicas
- Proyectos educativos
- Experimentos financieros

---

## 🛣️ Roadmap

Funciones previstas para futuras versiones:

- [ ] Persistencia en archivos
- [ ] Historial de transacciones
- [ ] Impuestos
- [ ] Mercado de recursos
- [ ] Intercambio entre monedas
- [ ] Soporte para múltiples bancos
- [ ] Intereses periódicos
- [ ] Estadísticas económicas

---

## 🤝 Contribuir

Las contribuciones, sugerencias y reportes de errores son bienvenidos.

Puedes abrir un Issue o enviar un Pull Request.

---

## 📄 Licencia

Este proyecto está licenciado bajo la licencia GNU Affero General Public License v3.0 (AGPL-3.0).

Esto significa que cualquier versión modificada distribuida o utilizada como servicio a través de una red debe poner a disposición su código fuente bajo la misma licencia.

---

Desarrollado con Python.
