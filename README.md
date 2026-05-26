# TrackAPI - GTM Templates

Dois templates para Google Tag Manager que simplificam a instalação do TrackAPI e garantem deduplicação correta entre Facebook browser Pixel e CAPI.

## Templates

| Arquivo | Tipo | Função |
|---|---|---|
| `trackapi-tag.tpl` | Tag | Carrega o SDK e inicializa o TrackAPI |
| `trackapi-event-id.tpl` | Variável | Gera event_id com cache para deduplicação |

## Instalação manual (sem galeria)

1. No GTM, vá em **Modelos → Novo → ⋮ → Importar**
2. Importe `template.tpl` → salve como **"TrackAPI Analytics"**
3. Importe o `template.tpl` do repo `trackapi-event-id` → salve como **"TrackAPI - Event ID"**

## Setup

### Tag TrackAPI Analytics
- Trigger: **All Pages**
- Project ID: `proj_xxx` (dashboard TrackAPI → Configurações)
- Endpoint: `https://analytics.seusite.com.br` (se tiver CNAME — recomendado)

### Deduplicação Facebook Pixel
Na tag do Facebook Pixel (nativa GTM):
- **Event ID** → `{{TrackAPI - Event ID}}`

## Fluxo de deduplicação

```
Usuário acessa a página
    ↓
GTM carrega
    ├─ Tag TrackAPI: carrega SDK → TrackAPI.init() → autoPageView → CAPI (event_id: evt_123)
    └─ Tag Facebook Pixel: fbq('track', 'PageView', {}, { eventID: {{TrackAPI - Event ID}} })
         └─ {{TrackAPI - Event ID}} retorna o mesmo evt_123 (cache 8s, mesma rota)
    ↓
Meta Events Manager: event_id idêntico nos dois canais → 1 conversão contabilizada ✅
```

## GTM Community Gallery

Este template está disponível na [Galeria de Templates do GTM](https://tagmanager.google.com/gallery/).

Para submeter uma atualização:
1. Atualize `template.tpl` e faça commit
2. Adicione o SHA do novo commit em `metadata.yaml` na seção `versions`
3. Faça push para o GitHub — atualizações aparecem na galeria em 2–3 dias
