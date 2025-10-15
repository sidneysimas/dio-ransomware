# dio-ransomware
Projeto Ransomware Dio

# Projeto CriptoFernet

## Descrição
Este é um script em Python que utiliza a biblioteca `cryptography.fernet` para criptografar arquivos de forma segura usando criptografia simétrica. Ideal para proteger dados sensíveis com uma chave gerenciada.

## Pré-requisitos
- Python 3.x
- Biblioteca `cryptography` (instalável via `pip`)

## Instalação
1. Clone o repositório:
   ```bash
   git clone https://github.com/sidneysimas/dio-ransomware.git

2. Instale as dependências:
   ```bash
   pip install cryptography

Uso
Como Executar
   1. Gere ou use a chave de criptografia (arquivo chave_fernet.key).
   2. Execute o script e informe o arquivo de entrada e saída:
  ```bash    
   python cripto_fernet.py


# Projeto CriptoFernet

## Descrição
Este é um script em Python que utiliza a biblioteca `cryptography.fernet` para criptografar arquivos.

## Código Fonte
```python
from cryptography.fernet import Fernet
import base64
import os

def gerar_ou_carregar_chave(caminho_chave="chave_fernet.key"):
    if os.path.exists(caminho_chave):
        with open(caminho_chave, "rb") as key_file:
            chave = key_file.read()
    else:
        chave = Fernet.generate_key()
        with open(caminho_chave, "wb") as key_file:
            key_file.write(chave)
    return chave

def criptografar_arquivo(arquivo_entrada, chave, arquivo_saida=None):
    try:
        f = Fernet(chave)
        with open(arquivo_entrada, "rb") as file:
            dados = file.read()
        dados_criptografados = f.encrypt(dados)
        if arquivo_saida:
            with open(arquivo_saida, "wb") as output_file:
                output_file.write(dados_criptografados)
            print(f"Arquivo criptografado salvo em: {arquivo_saida}")
        else:
            print("Dados criptografados gerados, mas não salvos. Retornando...")
        return dados_criptografados
    except FileNotFoundError:
        print(f"Erro: Arquivo {arquivo_entrada} não encontrado!")
        return None
    except Exception as e:
        print(f"Erro ao criptografar: {e}")
        return None

def main():
    caminho_chave = "chave_fernet.key"
    arquivo_entrada = input("Digite o caminho do arquivo a criptografar: ") or "entrada.txt"
    arquivo_saida = input("Digite o caminho para salvar o arquivo criptografado (deixe vazio para não salvar): ") or None
    chave = gerar_ou_carregar_chave(caminho_chave)
    print(f"Chave gerada/carregada: {base64.urlsafe_b64encode(chave).decode()}")
    resultado = criptografar_arquivo(arquivo_entrada, chave, arquivo_saida)
    if resultado:
        print("Criptografia concluída com sucesso!")
        if not arquivo_saida:
            print(f"Dados criptografados (base64): {base64.urlsafe_b64encode(resultado).decode()}")

if __name__ == "__main__":
    main()
```
