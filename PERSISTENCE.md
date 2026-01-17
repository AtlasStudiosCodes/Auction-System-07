# Sistema de Persistência e Versionamento - Auction Bot

## ✨ Novas Funcionalidades

### 1. **Sistema de Persistência de Dados**
O bot agora salva automaticamente todos os dados (auctions, trades e inventários) em um arquivo JSON.

#### Características:
- ✅ **Salvamento Automático**: Dados são salvos a cada 5 minutos
- ✅ **Recuperação ao Iniciar**: Quando o bot reinicia, carrega os dados anteriores
- ✅ **Salvamento ao Desligar**: Ao encerrar o bot, salva todos os dados atuais
- ✅ **Sem Perda de Dados**: Trades, auctions e inventários dos usuários são preservados

#### Arquivos Gerados:
- `data.json` - Armazena todos os auctions, trades, inventários e contadores
- `version.json` - Rastreia as versões de cada sistema

### 2. **Sistema de Versionamento Automático**
A versão dos sistemas é atualizada automaticamente quando há mudanças significativas.

#### Como Funciona:
- **Versão Inicial**: Cada sistema começa com uma versão base (ex: 1.1.2)
- **Incremento Automático**: Sempre que há uma mudança, o patch (terceira versão) é incrementado
  - Auction: Criação de novo auction → versão atualizada
  - Trade: Criação de novo trade → versão atualizada
  - Inventory: Criação/atualização de inventário → versão atualizada
  - Bid: Novo bid/lance → versão atualizada
- **Formato**: `major.minor.patch` (ex: 1.1.2 → 1.1.3)

#### Embeds Dinâmicos:
Os footers dos embeds agora mostram a versão atual do sistema em tempo real:
```
Version ${getVersion('auction')} | Made By Atlas
```

### 3. **Estrutura de Dados**

#### data.json
```json
{
  "auctions": [
    {
      "channelId": "...",
      "host": { "id": "...", "username": "..." },
      "bids": [...],
      ...
    }
  ],
  "trades": [...],
  "inventories": [...],
  "userTradeCount": { "userId": count, ... },
  "lastSaved": "2026-01-17T12:00:00Z"
}
```

#### version.json
```json
{
  "auction": "1.1.3",
  "trade": "1.1.4",
  "inventory": "1.1.1",
  "bid": "1.1.3",
  "lastUpdated": "2026-01-17T12:00:00Z"
}
```

## 📝 Como Usar

### Inicialização Normal
```bash
npm start
```
- Bot carrega dados anteriores automaticamente
- Salva dados a cada 5 minutos
- Salva ao desligar gracefully (Ctrl+C)

### Limpeza Manual de Dados
Para limpar todos os dados salvos:
```bash
rm data.json
```

## 🛡️ Tratamento de Graceful Shutdown
O bot responde aos sinais:
- `SIGINT` (Ctrl+C)
- `SIGTERM` (Encerramento normal)

Em ambos os casos, antes de desligar:
1. Salva todos os dados em `data.json`
2. Desconecta do Discord
3. Encerra o processo

## 🔧 Função de Versionamento

### `updateVersion(system)`
Incrementa a versão de um sistema específico.

**Parâmetros:**
- `'auction'` - Sistema de leilões
- `'trade'` - Sistema de trocas
- `'inventory'` - Sistema de inventário
- `'bid'` - Sistema de lances

**Exemplo:**
```javascript
updateVersion('auction'); // 1.1.2 → 1.1.3
```

### `getVersion(system)`
Retorna a versão atual de um sistema.

**Retorno:** String (ex: "1.1.3")

## 💾 Funções de Persistência

### `saveData()`
Salva todos os dados em `data.json`
- Chamada automaticamente a cada 5 minutos
- Chamada ao desligar o bot
- Pode ser chamada manualmente se necessário

### `loadData()`
Carrega dados do `data.json`
- Chamada automaticamente ao iniciar
- Restaura auctions, trades, inventários e contadores

## ⚙️ Configuração

### Intervalo de Salvamento
Para alterar de 5 minutos para outro valor, edite:
```javascript
// Em bot.js, na função clientReady
setInterval(() => {
  saveData();
}, 5 * 60 * 1000); // Altere aqui (em millisegundos)
```

## 📊 Monitoramento

### Logs de Salvamento
O bot exibe logs quando salva dados:
```
✓ Data saved successfully
✓ auction version updated to 1.1.3
```

### Logs de Carregamento
Ao iniciar:
```
Loading previous data...
✓ Loaded 5 auctions
✓ Loaded 12 trades
✓ Loaded 8 inventories
```

## 🚀 Benefícios

1. **Sem Perda de Dados**: Nenhuma informação importante é perdida ao reiniciar
2. **Histórico de Versões**: Fácil rastreamento de quando mudanças ocorreram
3. **Transparência**: Usuários podem ver a versão do sistema
4. **Confiabilidade**: Dados são salvos regularmente e ao desligar
5. **Escalabilidade**: Sistema pronto para mais tipos de dados no futuro

---

**Última Atualização**: 17 de Janeiro, 2026
**Versão**: 1.1+ (com persistência)
