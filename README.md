#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

int main() {
   
    const int MAX_CONTAS = 100;
    int numeroConta[MAX_CONTAS];
    string nomeCliente[MAX_CONTAS];
    string cpf[MAX_CONTAS];
    int tipoConta[MAX_CONTAS];      
    double saldo[MAX_CONTAS];
    bool contaAtiva[MAX_CONTAS];
    int quantidadeContas = 0;

   
    const int MAX_MOVIMENTACOES = 500;
    int contaMovimentacao[MAX_MOVIMENTACOES];
    char tipoMovimentacao[MAX_MOVIMENTACOES];   
    double valorMovimentacao[MAX_MOVIMENTACOES];
    int quantidadeMovimentacoes = 0;

    cout << fixed << setprecision(2);

    int opcao;
    do {
        cout << "\n===== SISTEMA BANCARIO =====\n";
        cout << "1 - Cadastrar conta\n";
        cout << "2 - Listar contas\n";
        cout << "3 - Consultar conta\n";
        cout << "4 - Depositar\n";
        cout << "5 - Sacar\n";
        cout << "6 - Transferir\n";
        cout << "7 - Extrato\n";
        cout << "0 - Sair\n";
        cout << "Opcao: ";
        cin >> opcao;

       
        if (opcao == 1) {
            if (quantidadeContas >= MAX_CONTAS) {
                cout << "Limite maximo de contas atingido!\n";
            } else {
                int numero;
                cout << "Numero da conta: ";
                cin >> numero;

               
                bool existe = false;
                for (int i = 0; i < quantidadeContas; i++) {
                    if (numeroConta[i] == numero) {
                        existe = true;
                    }
                }

                if (numero <= 0) {
                    cout << "Numero de conta invalido!\n";
                } else if (existe) {
                    cout << "Ja existe uma conta com esse numero!\n";
                } else {
                    string nome, documento;
                    int tipo;
                    double saldoInicial;

                    cout << "Nome do cliente: ";
                    getline(cin >> ws, nome);
                    cout << "CPF: ";
                    getline(cin >> ws, documento);
                    cout << "Tipo da conta (1-Corrente, 2-Poupanca): ";
                    cin >> tipo;
                    cout << "Saldo inicial: R$ ";
                    cin >> saldoInicial;

                    if (tipo != 1 && tipo != 2) {
                        cout << "Tipo de conta invalido!\n";
                    } else if (saldoInicial < 0) {
                        cout << "Saldo inicial nao pode ser negativo!\n";
                    } else {
                        numeroConta[quantidadeContas] = numero;
                        nomeCliente[quantidadeContas] = nome;
                        cpf[quantidadeContas] = documento;
                        tipoConta[quantidadeContas] = tipo;
                        saldo[quantidadeContas] = saldoInicial;
                        contaAtiva[quantidadeContas] = true;
                        quantidadeContas++;
                        cout << "Conta cadastrada com sucesso!\n";
                    }
                }
            }
        }

       
        else if (opcao == 2) {
            if (quantidadeContas == 0) {
                cout << "Nenhuma conta cadastrada.\n";
            } else {
                cout << "\n--- Contas cadastradas ---\n";
                for (int i = 0; i < quantidadeContas; i++) {
                    cout << "Conta: " << numeroConta[i]
                         << " | Cliente: " << nomeCliente[i]
                         << " | CPF: " << cpf[i]
                         << " | Tipo: " << (tipoConta[i] == 1 ? "Corrente" : "Poupanca")
                         << " | Saldo: R$ " << saldo[i]
                         << " | " << (contaAtiva[i] ? "Ativa" : "Inativa") << endl;
                }
            }
        }

       
        else if (opcao == 3) {
            int numero;
            cout << "Numero da conta: ";
            cin >> numero;

            int indice = -1;
            for (int i = 0; i < quantidadeContas; i++) {
                if (numeroConta[i] == numero) {
                    indice = i;
                }
            }

            if (indice == -1) {
                cout << "Conta nao encontrada!\n";
            } else {
                cout << "\nConta: " << numeroConta[indice] << endl;
                cout << "Cliente: " << nomeCliente[indice] << endl;
                cout << "CPF: " << cpf[indice] << endl;
                cout << "Tipo: " << (tipoConta[indice] == 1 ? "Corrente" : "Poupanca") << endl;
                cout << "Saldo: R$ " << saldo[indice] << endl;
                cout << "Situacao: " << (contaAtiva[indice] ? "Ativa" : "Inativa") << endl;
            }
        }

        
        else if (opcao == 4) {
            int numero;
            double valor;
            cout << "Numero da conta: ";
            cin >> numero;
            cout << "Valor do deposito: R$ ";
            cin >> valor;

            int indice = -1;
            for (int i = 0; i < quantidadeContas; i++) {
                if (numeroConta[i] == numero) {
                    indice = i;
                }
            }

            if (indice == -1) {
                cout << "Conta nao encontrada!\n";
            } else if (!contaAtiva[indice]) {
                cout << "Conta inativa!\n";
            } else if (valor <= 0) {
                cout << "Valor deve ser positivo!\n";
            } else if (quantidadeMovimentacoes >= MAX_MOVIMENTACOES) {
                cout << "Historico de movimentacoes cheio!\n";
            } else {
                saldo[indice] = saldo[indice] + valor;
                contaMovimentacao[quantidadeMovimentacoes] = numeroConta[indice];
                tipoMovimentacao[quantidadeMovimentacoes] = 'D';
                valorMovimentacao[quantidadeMovimentacoes] = valor;
                quantidadeMovimentacoes++;
                cout << "Deposito realizado! Novo saldo: R$ " << saldo[indice] << endl;
            }
        }

       
        else if (opcao == 5) {
            int numero;
            double valor;
            cout << "Numero da conta: ";
            cin >> numero;
            cout << "Valor do saque: R$ ";
            cin >> valor;

            int indice = -1;
            for (int i = 0; i < quantidadeContas; i++) {
                if (numeroConta[i] == numero) {
                    indice = i;
                }
            }

            if (indice == -1) {
                cout << "Conta nao encontrada!\n";
            } else if (!contaAtiva[indice]) {
                cout << "Conta inativa!\n";
            } else if (valor <= 0) {
                cout << "Valor deve ser positivo!\n";
            } else if (valor > saldo[indice]) {
                cout << "Saldo insuficiente!\n";
            } else if (quantidadeMovimentacoes >= MAX_MOVIMENTACOES) {
                cout << "Historico de movimentacoes cheio!\n";
            } else {
                saldo[indice] = saldo[indice] - valor;
                contaMovimentacao[quantidadeMovimentacoes] = numeroConta[indice];
                tipoMovimentacao[quantidadeMovimentacoes] = 'S';
                valorMovimentacao[quantidadeMovimentacoes] = valor;
                quantidadeMovimentacoes++;
                cout << "Saque realizado! Novo saldo: R$ " << saldo[indice] << endl;
            }
        }

       
        else if (opcao == 6) {
            int numOrigem, numDestino;
            double valor;
            cout << "Conta de origem: ";
            cin >> numOrigem;
            cout << "Conta de destino: ";
            cin >> numDestino;
            cout << "Valor da transferencia: R$ ";
            cin >> valor;

            int indiceOrigem = -1, indiceDestino = -1;
            for (int i = 0; i < quantidadeContas; i++) {
                if (numeroConta[i] == numOrigem) {
                    indiceOrigem = i;
                }
                if (numeroConta[i] == numDestino) {
                    indiceDestino = i;
                }
            }

            if (indiceOrigem == -1 || indiceDestino == -1) {
                cout << "Uma das contas nao foi encontrada!\n";
            } else if (indiceOrigem == indiceDestino) {
                cout << "Origem e destino devem ser contas diferentes!\n";
            } else if (!contaAtiva[indiceOrigem] || !contaAtiva[indiceDestino]) {
                cout << "Ambas as contas devem estar ativas!\n";
            } else if (valor <= 0) {
                cout << "Valor deve ser positivo!\n";
            } else if (valor > saldo[indiceOrigem]) {
                cout << "Saldo insuficiente na conta de origem!\n";
            } else if (quantidadeMovimentacoes + 2 > MAX_MOVIMENTACOES) {
                cout << "Sem espaco no historico para registrar a transferencia!\n";
            } else {
                saldo[indiceOrigem] = saldo[indiceOrigem] - valor;
                saldo[indiceDestino] = saldo[indiceDestino] + valor;

        
                contaMovimentacao[quantidadeMovimentacoes] = numeroConta[indiceOrigem];
                tipoMovimentacao[quantidadeMovimentacoes] = 'S';
                valorMovimentacao[quantidadeMovimentacoes] = valor;
                quantidadeMovimentacoes++;

                
                contaMovimentacao[quantidadeMovimentacoes] = numeroConta[indiceDestino];
                tipoMovimentacao[quantidadeMovimentacoes] = 'D';
                valorMovimentacao[quantidadeMovimentacoes] = valor;
                quantidadeMovimentacoes++;

                cout << "Transferencia realizada com sucesso!\n";
                cout << "Saldo origem (" << numOrigem << "): R$ " << saldo[indiceOrigem] << endl;
                cout << "Saldo destino (" << numDestino << "): R$ " << saldo[indiceDestino] << endl;
            }
        }

        else if (opcao == 7) {
            int numero;
            cout << "Numero da conta: ";
            cin >> numero;

            int indice = -1;
            for (int i = 0; i < quantidadeContas; i++) {
                if (numeroConta[i] == numero) {
                    indice = i;
                }
            }

            if (indice == -1) {
                cout << "Conta nao encontrada!\n";
            } else {
                cout << "\n--- Extrato da conta " << numero << " (" << nomeCliente[indice] << ") ---\n";
                bool temMovimentacao = false;
                for (int i = 0; i < quantidadeMovimentacoes; i++) {
                    if (contaMovimentacao[i] == numero) {
                        temMovimentacao = true;
                        if (tipoMovimentacao[i] == 'D') {
                            cout << "Deposito: R$ " << valorMovimentacao[i] << endl;
                        } else if (tipoMovimentacao[i] == 'S') {
                            cout << "Saque: R$ " << valorMovimentacao[i] << endl;
                        }
                    }
                }
                if (!temMovimentacao) {
                    cout << "Nenhuma movimentacao registrada.\n";
                }
                cout << "Saldo atual: R$ " << saldo[indice] << endl;
            }
        }

        else if (opcao != 0) {
            cout << "Opcao invalida!\n";
        }

    } while (opcao != 0);

    cout << "Encerrando o sistema. Ate logo!\n";
    return 0;
}
