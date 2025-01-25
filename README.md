# **Testes Unitários em TypeScript**

Os **testes unitários** são uma prática essencial no desenvolvimento de software para garantir que cada unidade (função, método ou classe) funcione corretamente de forma isolada. O objetivo é detectar erros o mais cedo possível no ciclo de desenvolvimento.

---

## **Benefícios dos Testes Unitários**
1. **Evita regressões:** Assegura que mudanças no código não quebrem funcionalidades existentes.
2. **Facilita refatorações:** Proporciona confiança ao modificar o código.
3. **Melhora a qualidade do código:** Identifica problemas antes da entrega final.
4. **Documentação viva:** Serve como especificação de como as funções devem se comportar.

---

## **Ferramentas populares para testes unitários em TypeScript**
1. **Jest** – A ferramenta mais usada, fácil de configurar, com suporte a TypeScript.
2. **Mocha** + **Chai** – Uma combinação popular para testes assíncronos e assertion fluente.
3. **Vitest** – Alternativa rápida e moderna, compatível com Jest.
4. **Jasmine** – Framework simples para testes BDD.

---

## **1. Configurando o Jest com TypeScript**

### **Passo 1: Instalar dependências**
Execute o seguinte comando no seu projeto TypeScript:

```bash
npm init -y          # Inicializa o projeto
npm install --save-dev jest ts-jest @types/jest
```

### **Passo 2: Configurar o Jest para TypeScript**
Crie o arquivo de configuração `jest.config.js`:

```js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
};
```

### **Passo 3: Adicionar scripts de teste no `package.json`**
Edite seu `package.json` e adicione:

```json
"scripts": {
  "test": "jest"
}
```

---

## **2. Escrevendo o primeiro teste unitário com Jest**

Dado o seguinte código simples em **TypeScript**, no arquivo `math.ts`:

```typescript
export function sum(a: number, b: number): number {
    return a + b;
}

export function isEven(n: number): boolean {
    return n % 2 === 0;
}
```

Agora criamos o arquivo de teste **`math.test.ts`**:

```typescript
import { sum, isEven } from './math';

test('Deve somar dois números corretamente', () => {
    expect(sum(3, 5)).toBe(8);
    expect(sum(-1, 1)).toBe(0);
});

test('Deve verificar se um número é par', () => {
    expect(isEven(4)).toBe(true);
    expect(isEven(7)).toBe(false);
});
```

### **Executando os testes**
```bash
npm test
```

**Saída esperada:**
```
PASS  ./math.test.ts
✓ Deve somar dois números corretamente (5ms)
✓ Deve verificar se um número é par (3ms)
```

---

## **3. Estrutura básica de testes unitários com Jest**

Os testes são organizados em blocos:

- **`describe`**: Agrupa testes relacionados.
- **`test` ou `it`**: Define um caso de teste específico.
- **`expect`**: Define a expectativa do teste.

Exemplo de uso completo:

```typescript
describe('Funções Matemáticas', () => {
    test('Soma de dois números', () => {
        expect(sum(2, 2)).toBe(4);
    });

    test('Verificar número par', () => {
        expect(isEven(10)).toBe(true);
    });

    test('Verificar número ímpar', () => {
        expect(isEven(11)).toBe(false);
    });
});
```

---

## **4. Testando funções assíncronas**

Se sua função retornar uma `Promise`, você pode testá-la com **async/await** ou **.resolves/.rejects**:

### **Código fonte:**
```typescript
export async function fetchData(): Promise<string> {
    return new Promise((resolve) =>
        setTimeout(() => resolve("Dados carregados"), 1000)
    );
}
```

### **Arquivo de teste:**
```typescript
import { fetchData } from './math';

test('Deve retornar os dados esperados', async () => {
    await expect(fetchData()).resolves.toBe("Dados carregados");
});

test('Testando função assíncrona com async/await', async () => {
    const data = await fetchData();
    expect(data).toBe("Dados carregados");
});
```

---

## **5. Testando exceções e erros**

Caso uma função deva lançar erros, você pode testá-la usando `toThrow`:

### **Código fonte:**
```typescript
export function dividir(a: number, b: number): number {
    if (b === 0) {
        throw new Error("Divisão por zero não é permitida");
    }
    return a / b;
}
```

### **Arquivo de teste:**
```typescript
import { dividir } from './math';

test('Deve lançar erro ao dividir por zero', () => {
    expect(() => dividir(10, 0)).toThrow("Divisão por zero não é permitida");
});
```

---

## **6. Mocking em testes unitários**

Mocks permitem substituir dependências externas, como chamadas de API ou banco de dados, durante os testes.

### **Código fonte:**
```typescript
export class UserService {
    async getUser(id: number): Promise<string> {
        // Simula chamada a um banco de dados
        return `Usuário ${id}`;
    }
}
```

### **Arquivo de teste usando mocks:**
```typescript
import { UserService } from './user';

jest.mock('./user');

test('Deve mockar chamada de API', async () => {
    const mockGetUser = jest.spyOn(UserService.prototype, 'getUser');
    mockGetUser.mockResolvedValue('Usuário mockado');

    const userService = new UserService();
    const result = await userService.getUser(1);

    expect(result).toBe('Usuário mockado');
    expect(mockGetUser).toHaveBeenCalledWith(1);
});
```

---

## **7. Cobertura de testes (Test Coverage)**

Para medir a cobertura dos testes, execute:

```bash
npx jest --coverage
```

**Saída esperada:**
```
PASS  ./math.test.ts
----------------|---------|----------|---------|---------|
File            | % Stmts | % Branch | % Funcs | % Lines |
----------------|---------|----------|---------|---------|
All files       | 100.00  | 100.00   | 100.00  | 100.00  |
----------------|---------|----------|---------|---------|
```

---

## **8. Boas práticas para testes unitários**

1. **Escreva testes pequenos e independentes.**
2. **Cubra cenários positivos e negativos.**
3. **Evite dependências externas (banco de dados, APIs).**
4. **Use mocks e spies para isolar funcionalidades.**
5. **Mantenha uma nomenclatura clara nos testes.**
6. **Garanta uma cobertura adequada, sem exageros.**

---

## **9. Comparação entre Jest e Mocha/Chai**

| Característica   | Jest          | Mocha + Chai    |
|-----------------|---------------|----------------|
| Configuração    | Simples        | Mais complexa  |
| Suporte a TypeScript | Simples com `ts-jest` | Requer mais configuração |
| Mocking         | Embutido       | Precisa de sinon.js |
| Performance     | Alta           | Moderada       |
| Comunidade      | Grande         | Grande         |

---

## **Conclusão**

Testes unitários são uma parte essencial do desenvolvimento de software, ajudando a evitar erros e a melhorar a qualidade do código. Com ferramentas como **Jest**, é possível escrever testes de forma simples e eficaz em projetos TypeScript.

Se precisar de mais explicações ou exemplos práticos, estou à disposição!
