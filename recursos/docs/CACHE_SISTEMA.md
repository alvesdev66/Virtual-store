# 🚀 Sistema de Cache de Imagens - 10 Dias

## Visão Geral
Um sistema completo de cache de imagens usando **IndexedDB** com duração de **10 dias**. Reduz solicitações ao servidor ImageKit e evita atingir o limite de banda.

---

## 📋 Como Funciona

### 1. **Primeira Visita do Usuário**
- Quando um usuário acessa o site, o sistema verifica se a imagem está em cache
- Se **não estiver**, faz o download da URL do ImageKit
- **Imediatamente após o download**, a imagem é armazenada em cache (IndexedDB)
- A imagem é exibida no navegador

### 2. **Visitas Subsequentes (até 10 dias)**
- O sistema verifica automaticamente o cache (IndexedDB)
- Se a imagem estiver em cache **e não expirada**, usa a imagem local
- **Nenhuma solicitação é feita ao ImageKit**
- Carregamento muito mais rápido ⚡

### 3. **Após 10 Dias**
- O cache expira automaticamente
- Na próxima visualização, a imagem é baixada novamente do ImageKit
- E armazenada em cache por mais 10 dias

---

## 🔧 Detalhes Técnicos

### Classe: `ImagemCacheManager`

#### Propriedades:
```javascript
- dbName: 'JuveleCacheDB'
- storeName: 'imagensProdutos'
- DURACAO_CACHE: 864000000 ms (10 dias)
- db: Instância do IndexedDB
```

#### Métodos Principais:

1. **`obterImagemComCache(url)`**
   - Verifica se a imagem está em cache
   - Se estiver e não expirou: retorna blob URL do cache
   - Se não estiver: baixa do ImageKit, cacheia e retorna blob URL

2. **`salvarNoCache(url, blob)`**
   - Armazena a imagem em IndexedDB com timestamp

3. **`obterDoCache(url)`**
   - Recupera imagem do cache
   - Verifica expiração automaticamente

4. **`limparCacheExpirado()`**
   - Remove imagens que expiraram (> 10 dias)
   - Executada automaticamente ao iniciar

5. **`obterTamanhoCacheEmMB()`**
   - Retorna o tamanho total do cache em MB

---

## 🎯 Benefícios

✅ **Reduz banda de servidor**: Elimina múltiplas requisições à mesma imagem
✅ **Carregamento mais rápido**: Blob URLs carregam instantaneamente do disco local
✅ **Economia de dados**: Especialmente importante para usuários com conexão lenta
✅ **Limpeza automática**: Cache expirado é removido automaticamente
✅ **Sem limite de armazenamento**: IndexedDB oferece vários GB de espaço

---

## 🛠️ Ferramentas de Gerenciamento

### Acessar pelo Console do Navegador (F12)

```javascript
// Ver tamanho atual do cache
await window.JuveleCacheTools.tamanho();
// Saída: 📦 Tamanho total do cache: 45.23 MB

// Ver informações detalhadas
await window.JuveleCacheTools.info();
// Mostra tabela com URL, tamanho, data de armazenamento e expiração

// Limpar todo o cache manualmente
await window.JuveleCacheTools.limparTudo();
// Saída: ✅ Cache de imagens limpo com sucesso!

// Limpar apenas cache expirado
await window.JuveleCacheTools.limparExpirado();
// Saída: ✅ Cache expirado limpo com sucesso!
```

---

## 📊 Logs do Sistema

O sistema exibe logs informativos no console:

```
✅ Usando imagem do cache: https://ik.imagekit.io/...
📥 Baixando imagem para cache: https://ik.imagekit.io/...
✅ Imagem carregada: https://ik.imagekit.io/...
🧹 Limpeza de cache: 5 imagens expiradas removidas
```

---

## 💾 Armazenamento

### IndexedDB Structure:
```
Database: JuveleCacheDB
Object Store: imagensProdutos
  - url (key): URL da imagem do ImageKit
  - blob: Dados binários da imagem
  - timestamp: Data/hora do armazenamento
  - Index: timestamp (para buscas rápidas)
```

### Espaço Estimado:
- Firefox: até 10% do espaço livre em disco
- Chrome: até 50% do espaço livre em disco
- Safari: até 50% do espaço livre em disco

---

## 🔍 Monitoramento

### Logs Automáticos no Console:
1. Ao iniciar a página:
   ```
   ✅ Sistema de Cache Iniciado
   📦 Cache atual: 45.23 MB | Duração: 10 dias
   💡 Use window.JuveleCacheTools.info() para ver detalhes
   ```

2. Ao carregar cada imagem:
   ```
   ✅ Usando imagem do cache: [URL truncada]
   ou
   📥 Baixando imagem para cache: [URL truncada]
   ```

3. Ao limpar cache expirado:
   ```
   🧹 Limpeza de cache: 5 imagens expiradas removidas
   ```

---

## ⚙️ Integração com Sistema Existente

O cache foi integrado automaticamente em:

1. **Lazy Loading**: `iniciarLazyLoading()`
   - Carrega imagens do cache quando visíveis
   - Faz novo download apenas se não em cache

2. **Retry de Imagens**: `tentarRecarregarImagem()`
   - Tenta usar cache antes de fazer novo download
   - Respeita tentativas progressivas com delays

3. **Monitoramento**: `monitorarCardsComFalha()`
   - Verifica integridade de imagens em cache

---

## 🚨 Tratamento de Erros

O sistema trata automaticamente:
- Falha de fetch → Tenta cache
- Cache corrompido → Remove e redownload
- IndexedDB indisponível → Usa fallback (sem cache)
- Quota excedida → Limpa cache expirado

---

## 📱 Compatibilidade

✅ Chrome/Edge (v≥ 24)
✅ Firefox (v≥ 10)
✅ Safari (v≥ 10)
✅ Opera (v≥ 15)
✅ Mobile browsers (iOS Safari, Chrome Mobile)

⚠️ IE 11: Sem suporte IndexedDB avançado, usa fallback

---

## 🎬 Próximos Passos

### Opcional - Adicionar Service Worker (para ainda mais performance):
```javascript
// Pode ser adicionado para cache offline completo
```

### Opcional - Pré-carregamento de Cache:
```javascript
// Pode pré-cachear imagens antes do usuário visitá-las
```

---

## 📞 Suporte

Se encontrar problemas, verifique no console:
```javascript
// Veja informações detalhadas do cache
await window.JuveleCacheTools.info();

// Limpe e reinicie
await window.JuveleCacheTools.limparTudo();
location.reload();
```

---

**Desenvolvido em:** 29 de Janeiro de 2026
**Versão:** 1.0
**Duração:** 10 dias
