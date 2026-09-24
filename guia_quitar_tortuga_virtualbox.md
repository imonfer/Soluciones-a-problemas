# 🐢 Cómo quitar la tortuga de VirtualBox y recuperar el rendimiento

> **Guía rápida para Windows** · Tiempo estimado: 5 minutos

---

## ❓ ¿Qué es la tortuga?

Si en la barra de estado de tu máquina virtual (abajo a la derecha de la ventana) ves un **icono de tortuga verde**, significa que VirtualBox está funcionando en **modo de compatibilidad con Hyper-V**.

Esto ocurre cuando Windows tiene activadas ciertas funciones de virtualización (Hyper-V, WSL2, Windows Sandbox, Integridad de memoria...). En ese modo, VirtualBox no puede usar directamente el procesador y la máquina virtual va **notablemente más lenta**.

**Objetivo de esta guía:** desactivar esas funciones para que la tortuga sea sustituida por un **icono de chip** (VT-x/AMD-V) y la VM vaya fluida.

---

## ⚠️ Antes de empezar

- Mientras esto esté desactivado, **no funcionarán** WSL2, Docker Desktop ni Windows Sandbox.
- Necesitas **permisos de administrador** en el equipo.
- Guarda tu trabajo: al final hay que **reiniciar el ordenador**.

---

## 📋 Pasos

### 1️⃣ Apaga la máquina virtual

Apaga la VM desde dentro (no la dejes en pausa) y cierra también la ventana de VirtualBox. Así evitas problemas al cambiar la configuración de Windows.

### 2️⃣ Abre las características de Windows

1. Pulsa las teclas **`Windows` + `R`** a la vez.
2. Escribe `optionalfeatures` y pulsa **Enter**.
3. Se abrirá la ventana *«Activar o desactivar las características de Windows»*.

### 3️⃣ Desmarca las casillas de virtualización

En la lista, **quita la marca** de estas opciones (solo las que veas en tu equipo):

- [ ] Hyper-V
- [ ] Plataforma de máquina virtual
- [ ] Plataforma del hipervisor de Windows
- [ ] Windows Sandbox
- [ ] Subsistema de Windows para Linux

Pulsa **Aceptar**. Si te pide reiniciar, elige **«No reiniciar ahora»** (lo haremos al final).

> 💡 Si alguna casilla no aparece en tu equipo, es normal. Desmarca solo las que veas.

### 4️⃣ Desactiva la Integridad de memoria

1. Abre el menú **Inicio** y escribe **«Seguridad de Windows»**.
2. Entra en **Seguridad del dispositivo**.
3. Pulsa en **Aislamiento del núcleo** → **Detalles del aislamiento del núcleo**.
4. Pon el interruptor de **«Integridad de memoria»** en **Desactivado**.

### 5️⃣ Ejecuta el comando de bcdedit

1. Abre el menú **Inicio** y escribe `cmd`.
2. Haz **clic derecho** en *«Símbolo del sistema»* y elige **«Ejecutar como administrador»**.
3. Escribe exactamente este comando y pulsa **Enter**:

```bat
bcdedit /set hypervisorlaunchtype off
```

Debe aparecer el mensaje: *«La operación se completó correctamente»*.

### 6️⃣ Reinicia el ordenador

Reinicia Windows por completo (**Inicio → Reiniciar**). Este paso es imprescindible para que los cambios se apliquen.

### 7️⃣ Comprueba que funcionó ✅

1. Abre **VirtualBox** e inicia tu máquina virtual.
2. Mira la barra de estado de abajo a la derecha.
3. La **tortuga** 🐢 debería haber sido sustituida por un **icono de chip** y la VM debería ir bastante más fluida.

---

## 🔄 ¿Cómo volver atrás?

Si necesitas usar WSL2, Docker u otras funciones de Hyper-V otra vez:

1. Abre **cmd como administrador** y ejecuta:

   ```bat
   bcdedit /set hypervisorlaunchtype auto
   ```

2. Vuelve a marcar las características que quitaste en el paso 3 (`optionalfeatures`).
3. Reactiva la **Integridad de memoria** si la desactivaste (paso 4).
4. Reinicia el ordenador.

---

## 🧰 ¿Sigue apareciendo la tortuga?

Puede que otra función siga activando el hipervisor. Prueba a revisar:

| Posible causa | Qué hacer |
|---|---|
| Docker Desktop o WSL2 instalados | Ciérralos o desinstálalos, y ejecuta `wsl --shutdown` en cmd |
| *Protección de aplicaciones de Microsoft Defender* | Desmárcala en las características de Windows |
| Integridad de memoria sigue activa | Revisa el paso 4 (a veces Windows la reactiva tras actualizar) |
| Virtualización desactivada en la BIOS/UEFI | Actívala (busca *Intel VT-x* o *AMD-V / SVM Mode*) |

---

## 📝 Resumen rápido

| Paso | Acción |
|:---:|---|
| 1 | Apagar la VM |
| 2 | `Win + R` → `optionalfeatures` |
| 3 | Desmarcar Hyper-V, Plataforma de máquina virtual, Plataforma del hipervisor, Sandbox y WSL |
| 4 | Desactivar *Integridad de memoria* |
| 5 | `bcdedit /set hypervisorlaunchtype off` (cmd como administrador) |
| 6 | Reiniciar |
| 7 | Comprobar que aparece el chip en lugar de la tortuga |

---

*¡Listo! Si algo no funciona, compártelo con el grupo y lo revisamos entre todos.* 🚀
