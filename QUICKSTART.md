# 🎯 Guia Rápido - Persistência e Versionamento

## 📱 Para Usuários

### Iniciar o Bot
```bash
npm start
```

### O que acontece:
1. ✅ Bot carrega auctions, trades e inventários anteriores
2. ✅ Dados são salvos automaticamente a cada 5 minutos
3. ✅ Ao encerrar (Ctrl+C), dados são salvos

## 👨‍💻 Para Desenvolvedores

### Ver Versão de um Sistema
```javascript
console.log(getVersion('auction')); // "1.1.3"
```

### Atualizar Versão Manualmente
```javascript
updateVersion('auction'); // 1.1.3 → 1.1.4
```

### Salvar Dados Manualmente
```javascript
saveData();
```

### Carregar Dados Manualmente
```javascript
loadData();
```

---

## 📂 Arquivos Importantes

```
/workspaces/Auction-System-03/
├── bot.js                    # Bot principal (modificado)
├── data.json                 # Dados persistidos ✨ NOVO
├── version.json              # Versões dos sistemas ✨ NOVO
├── PERSISTENCE.md            # Documentação completa ✨ NOVO
├── CHANGELOG.md              # Histórico de mudanças ✨ NOVO
└── test-setup.sh             # Script de teste ✨ NOVO
```

---

## 🔧 Configuração Avançada

### Alterar Intervalo de Salvamento

Em `bot.js`, linha ~330:
```javascript
// Padrão: 5 minutos (300,000 ms)
setInterval(() => {
  saveData();
}, 5 * 60 * 1000); // ← Altere aqui

// Exemplo: Salvar a cada 1 minuto
setInterval(() => {
  saveData();
}, 1 * 60 * 1000);
```

### Backup Manual
```bash
# Fazer backup dos dados
cp data.json data.backup.json
cp version.json version.backup.json

# Restaurar do backup
cp data.backup.json data.json
cp version.backup.json version.json
```

---

## 🧪 Testar o Sistema

```bash
# Executar testes
./test-setup.sh

# Esperado:
# ✓ Arquivo data.json encontrado
# ✓ Arquivo version.json encontrado
# ✓ Sintaxe OK
```

---

## 🆘 Troubleshooting

### Problema: "data.json not found"
**Solução**: Na primeira execução, será criado automaticamente

### Problema: "Cannot read property 'length' of undefined"
**Solução**: Deletar data.json e reiniciar
```bash
rm data.json
npm start
```

### Problema: "Invalid JSON in data.json"
**Solução**: Restaurar do backup ou deletar
```bash
rm data.json
```

### Problema: "Bot não está salvando dados"
**Solução**: Verificar logs
```bash
npm start 2>&1 | grep "saved\|loaded"
```

---

## 📈 Fluxo de Dados

```
INICIALIZAÇÃO:
  ↓
  loadData() → restaura auctions/trades/inventários
  ↓
  Bot pronto para usar

DURANTE EXECUÇÃO:
  ↓
  Usuário cria/atualiza (auction/trade/bid/inventory)
  ↓
  updateVersion(sistema) → incrementa versão
  ↓
  Dados salvos em tempo real na Map em memória

A CADA 5 MINUTOS:
  ↓
  setInterval → saveData()
  ↓
  Maps convertidas em Arrays
  ↓
  Escrito em data.json

ENCERRAMENTO:
  ↓
  SIGINT/SIGTERM recebido
  ↓
  saveData() → salva último estado
  ↓
  client.destroy()
  ↓
  process.exit(0)
```

---

## 📊 Estrutura de Dados Salva

### Auctions
```javascript
{
  channelId: "...",
  messageId: "...",
  host: { id: "...", username: "..." },
  title: "...",
  description: "...",
  bids: [
    { user: {...}, diamonds: 1000, items: "...", timestamp: 123456 },
    ...
  ],
  ...
}
```

### Trades
```javascript
{
  messageId: "...",
  channelId: "...",
  host: { id: "...", username: "..." },
  hostDiamonds: 500,
  hostItems: [...],
  offers: [
    { user: {...}, diamonds: 600, items: [...], timestamp: 123456 },
    ...
  ],
  ...
}
```

### Inventories
```javascript
{
  userId: "...",
  messageId: "...",
  channelId: "...",
  items: [{ name: "...", quantity: 5 }, ...],
  diamonds: 1000,
  lookingFor: "...",
  robloxUsername: "...",
  lastEdited: "2026-01-17T12:00:00Z"
}
```

---

## ✅ Checklist de Implementação

- ✅ Sistema de persistência implementado
- ✅ Sistema de versionamento automático implementado
- ✅ Salvamento automático a cada 5 minutos
- ✅ Carregamento ao iniciar
- ✅ Salvamento ao desligar gracefully
- ✅ Todos os warnings de deprecação removidos
- ✅ Todos os await adicionados
- ✅ Documentação completa
- ✅ Scripts de teste

---

**Pronto para usar!** 🚀

Para dúvidas, consulte `PERSISTENCE.md` ou `CHANGELOG.md`
