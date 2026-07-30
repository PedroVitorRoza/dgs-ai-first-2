# AGENTS.md

## Testing Standards

Todos os testes gerados pelos agentes de IA devem seguir os padrões abaixo para manter consistência e facilitar a revisão pelo time de QA.

### Nomenclatura

Utilizar `describe` e `it` com frases descritivas em inglês.

Exemplo:

```ts
describe('QueryHandler', () => {
  it('should return the correct answer when the query is valid')
})
```

Evitar nomes genéricos como "test endpoint" ou "works".

---

### Todo teste DEVE conter

- Estrutura clara de Arrange, Act e Assert.
- Apenas uma responsabilidade por teste.
- Assertions específicas sobre o comportamento esperado.
- Dados previsíveis e reutilizáveis.
- Independência em relação aos demais testes.

---

### Todo teste NÃO DEVE conter

- Chamadas para APIs ou serviços reais.
- Dependência da ordem de execução.
- Assertions genéricas como `toBeDefined()` ou `toBeTruthy()` sozinhas.
- Valores fixos espalhados pelo código.
- Dependência de dados externos.

---

### Mocking

- Utilizar **MSW (Mock Service Worker)** para chamadas HTTP.
- Criar factories para gerar objetos utilizados nos testes.
- Evitar mocks repetidos dentro de cada teste.

---

### Fixtures

Os dados reutilizáveis devem ficar na pasta:

```
tests/fixtures/
```

Ela deverá conter:

- perguntas de exemplo;
- chunks recuperados pelo RAG;
- respostas esperadas;
- documentos simulados da NovaTech.

---

### Exemplo

#### Antes

```ts
test('query endpoint works', async () => {
  const result = await handler({ body: '{"question":"test"}' });

  expect(result).toBeDefined();
});
```

#### Depois

```ts
describe('QueryHandler', () => {
  it('should return the source document when a valid question is received', async () => {
    // Arrange
    const request = {
      body: '{"question":"Qual o prazo de entrega padrão?"}'
    };

    // Act
    const result = await handler(request);

    // Assert
    expect(result.statusCode).toBe(200);
    expect(result.body).toContain('source_document');
    expect(result.body).toContain('PROC-042');
  });
});
```

### Melhorias realizadas

- Nome do teste descreve exatamente o comportamento esperado.
- Estrutura organizada em Arrange, Act e Assert.
- Assertions verificam informações relevantes.
- Utiliza um cenário realista.
- Facilita manutenção e entendimento.

---

### Critérios para aprovação no Code Review

Um teste gerado por IA será aprovado apenas se atender aos seguintes critérios:

- Utiliza Arrange, Act e Assert de forma clara.
- Possui assertions específicas relacionadas ao comportamento esperado.
- Não realiza chamadas para serviços externos.
- Utiliza mocks e fixtures definidos pelo projeto.
- Possui nome descritivo em inglês.
