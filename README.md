# Larastan and Pint - Guia de Instalação

Este guia fornece instruções detalhadas para instalar e configurar o Larastan (análise estática de código) e o Laravel Pint (formatação de código) em seu projeto Laravel.

## 📦 Instalação

### Passo 1: Instalar os pacotes

Primeiro, instale ambos os pacotes como dependências de desenvolvimento usando o Composer:

```bash
composer require --dev "larastan/larastan:^3.0"
composer require --dev "laravel/pint"
```

**Nota:** Certifique-se de que você está no diretório raiz do seu projeto Laravel antes de executar estes comandos.

## ⚙️ Configuração

### Configurando o Larastan

Após a instalação do Larastan, crie um arquivo de configuração `phpstan.neon` na raiz do seu projeto:

```bash
touch phpstan.neon
```

Adicione o seguinte conteúdo ao arquivo `phpstan.neon`:

```neon
includes:
    - vendor/larastan/larastan/extension.neon

parameters:
    paths:
        - app/
    
    # Nível de análise (0-9, sendo 9 o mais rigoroso)
    level: 5
    
    # Ignora erros em arquivos específicos (opcional)
    excludePaths:
        - bootstrap/cache
```

### Configurando o Pint

O Laravel Pint funciona sem configuração, mas você pode personalizar as regras criando um arquivo `pint.json`:

```bash
touch pint.json
```

Exemplo de configuração do `pint.json`:

```json
{
    "preset": "laravel",
    "rules": {
        "array_syntax": {
            "syntax": "short"
        },
        "binary_operator_spaces": {
            "default": "single_space"
        },
        "blank_line_after_namespace": true,
        "blank_line_after_opening_tag": true,
        "blank_line_before_statement": {
            "statements": ["return"]
        },
        "concat_space": {
            "spacing": "one"
        }
    }
}
```

## 🚀 Uso

### Usando o Larastan

Execute a análise estática do código:

```bash
# Análise completa
./vendor/bin/phpstan analyse

# Análise com nível específico
./vendor/bin/phpstan analyse --level=max

# Análise de arquivos específicos
./vendor/bin/phpstan analyse app/Http/Controllers
```

### Usando o Pint

Execute a formatação de código:

```bash
# Formatar todo o código
./vendor/bin/pint

# Modo dry-run (apenas visualizar mudanças sem aplicar)
./vendor/bin/pint --test

# Formatar arquivos específicos
./vendor/bin/pint app/Http/Controllers

# Formatar com verbose
./vendor/bin/pint -v
```

## 📝 Scripts do Composer (Opcional)

Você pode adicionar scripts ao seu `composer.json` para facilitar a execução:

```json
{
    "scripts": {
        "analyse": "phpstan analyse",
        "format": "pint",
        "format:test": "pint --test"
    }
}
```

Então execute com:

```bash
composer analyse
composer format
composer format:test
```

## 🔍 Integração com CI/CD

### GitHub Actions

Exemplo de workflow `.github/workflows/code-quality.yml`:

```yaml
name: Code Quality

on: [push, pull_request]

jobs:
  code-quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.2'
          
      - name: Install Dependencies
        run: composer install --prefer-dist --no-progress
        
      - name: Run Pint
        run: ./vendor/bin/pint --test
        
      - name: Run Larastan
        run: ./vendor/bin/phpstan analyse
```

## 📚 Recursos Adicionais

- **Larastan:** [https://github.com/larastan/larastan](https://github.com/larastan/larastan)
- **Laravel Pint:** [https://laravel.com/docs/pint](https://laravel.com/docs/pint)
- **PHPStan:** [https://phpstan.org/](https://phpstan.org/)

## 💡 Dicas

1. **Incremente gradualmente o nível do Larastan:** Comece com nível 0 ou 1 e vá aumentando conforme corrige os erros.
2. **Execute o Pint antes de commits:** Mantenha seu código sempre formatado.
3. **Use pre-commit hooks:** Configure hooks Git para executar automaticamente essas ferramentas.
4. **Ignore arquivos gerados:** Adicione pastas como `bootstrap/cache` e `storage` ao `excludePaths` do Larastan.

## 📄 Licença

Este guia é de uso livre para a comunidade Laravel.