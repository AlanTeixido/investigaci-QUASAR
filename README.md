# 🛠️ Manual Tècnic — Activitat d'Investigació Quasar

## 📦 1. Components de Quasar utilitzats

A continuació es detalla cada component utilitzat, el seu propòsit, props, events i raó de selecció:

### 🔹 QTable
- **Funció**: Mostrar la llista d'usuaris de l'API
- **Motiu**: Component potent per mostrar llistes amb format taula, fàcilment estilitzable
- **Props**:
  - `rows`: dades a mostrar
  - `columns`: definició de columnes
  - `row-key`: identificador únic (id)
- **Events**:
  - `@row-click`: per navegar al detall
- **Slots**:
  - `body-cell-actions`: per injectar botons d'acció

### 🔹 QInput
- **Funció**: Camp de cerca i inputs en formularis
- **Motiu**: Integració directa amb v-model, ràpid i estètic
- **Props**:
  - `v-model`
  - `label`, `debounce`, `filled`, `type`

### 🔹 QBtn
- **Funció**: Botons d'acció com "Enviar", "Eliminar", "Editar"
- **Props**:
  - `label`, `color`, `type`, `icon`, `flat`, `round`
- **Events**:
  - `@click`: accions específiques

### 🔹 QDialog
- **Funció**: Confirmar eliminació d'un usuari
- **Motiu**: UX millorada, evita eliminacions accidentals
- **Props**:
  - `v-model`: controla obertura
- **Slots**:
  - `q-card-section`, `q-card-actions`

### 🔹 QCard
- **Funció**: Mostrar dades del detall i contingut dels diàlegs
- **Props**:
  - No essencials, però combinat amb `q-card-section` i `q-card-actions`

### 🔹 QForm, QSelect
- **Funció**: Formularis reactius, selecció d'empresa
- **Props**: `options`, `v-model`, `rules`, `emit-value`

### 🔹 Notify (plugin)
- **Funció**: Feedback visual (usuari creat, eliminat, errors...)
- **Ús**: `Notify.create({ message: '...', color: '...' })`

## 🌐 2. Documentació de l'API

- **API utilitzada**: JSONPlaceholder

### 🔸 Endpoints

| **Mètode** | **URL** | **Descripció** |
|------------|---------|---------------|
| GET | `/users` | Obtenir tots els usuaris |
| GET | `/users/:id` | Obtenir un usuari pel seu ID |

### 🔸 Exemple de peticions

```js
// Obtenir tots els usuaris
await axios.get('https://jsonplaceholder.typicode.com/users')
// Obtenir un usuari pel seu ID
await axios.get(`https://jsonplaceholder.typicode.com/users/${id}`)
```

### 🔸 Exemple de resposta

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "email": "Sincere@april.biz",
  "company": { "name": "Romaguera-Crona" }
}
```

## 🔁 3. Flux de navegació de l'aplicació

### 🔸 Estructura general

```yaml
[HomePage.vue]
 ├── Llista d'usuaris (QTable)
 ├── Cerca amb QInput
 ├── Accions: Editar / Eliminar
 ├── Navega a → [UserDetail.vue]
[UserDetail.vue]
 └── Mostra dades detallades de l'usuari
[FormUser.vue]
 ├── Si id present a query → mode edició
 ├── Si no → mode creació
 ├── Enviar = Notificació + redirecció
```

### 🔸 Consum de dades
- Es fa amb axios a cada vista:
  - `onMounted()` → `axios.get(...)`
  - `axios.post(...)` i `axios.put(...)` simulats
  - Eliminació amb filtre de l'array local (`users.value = users.value.filter(...)`)

### 🔸 Funcionalitats implementades

| **Funció** | **Implementada** |
|------------|-----------------|
| Llistat d'usuaris | ✅ |
| Cerca en temps real | ✅ |
| Vista detall | ✅ |
| Crear nou usuari | ✅ (simulat) |
| Editar usuari | ✅ (simulat, amb route.query.id) |
| Eliminar usuari | ✅ (amb QDialog) |
| Validació de formulari | ✅ amb rules |
| Feedback visual | ✅ amb Notify |

## ✅ Conclusió

L'aplicació compleix tots els requisits de l'activitat:
- Ús adequat i variat de components de Quasar
- Navegació amb router dinàmic
- Consum d'API real i simulació de CRUD
- Bona organització i estil consistent
