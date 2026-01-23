# Sistemas em lote (batch) PT.1 

Antigamente as computadores eram máquinas exageradamente grandes e eram operadas por um console. Os dispositivos de entrada comuns eram leitores de cartões e unidades de fita, os dispositivos de saída eram impressoras de linhas, unidades de fita e perfuradores de cartões.

<img src="./images/computador_antigo.jpeg" width="650px">

O usuário não interagia diretamente com a máquina. Em vez disso ele preparava um `job (tarefa)` para que o computador executasse.

O Sistema Operacional nessa época era bem simples, sua principal função era transmitir o controle de um `job` para o seguinte. Os operadores começaram a agrupar `jobs` com necessidades parecidas para os executar em grupo ou `batch`, aceleando o processamento e se tornando mais eficiênte, na teoria...

Exemplo em Go:

```go
type Writer interface {
	Write()
}

type Job1 struct {}

type Job2 struct {}

func (Job1) Write() {
	// escreve...
}

func (Job2) Write() {
	// escreve...
}
// passa os jobs para que o OS execute
func OS(jobs ...Writer) {
	// executa cada job passado
	for _, job := range jobs {
		Execute(job)
	}
}
```
> Aqui basicamente `Job1` e `Job2` são tarefas a serem executadas tendo suas funções `Write()`, a `interface` chamada `Writer` define que se um objeto tiver a função `Write()` ele será um `Writer`, nesse caso o Sistema Operacional está sendo representado como uma função `OS()` e ela pode receber inúmeros jobs com as mesmas características, no final cada jobs é executado um por vez.       

Exercendo essa prática as CPUs ficavam ociosas pelo fato de que os dispositivos mecânicos (I/O nesse caso) eram extremamente mais lentos do que os dispositivos eletrônicos (CPU). Mesmo uma CPU lenda ainda sim era capaz de executar milhões de instruções por segundo.

Com os avanços tecnológicos e melhorias resultou em dispositivos I/O mais rápidos, porém a velocidade das CPUs também aumentaram, com isso o problema não só não foi resolvido mas também foi exarcebado.

## A Chegada dos Discos

A chegada dos discos permitiu com que o sistema operacional pudesse manter os `jobs` em disco, antes ficava em uma **leitora de cartões serial¹**. Com isso o **escalonamento de jobs²** poderia ser usado para realizar tarefas de forma mais eficiente.

Por falar em eficiência, o aspecto mais importante do escalonamento de jobs é a capacidade de `multiprogramação`.

Um único usuário não pode, na maioria dos casos, manter a CPU ou os dois dispositivos I/O ocupados em todos os momentos. A multiprogramação aumenta a utilização da CPU organizando os jobs de forma que a CPU sempre tenha um job para executar, consequêntemente evitando de deixar a CPU ociosa.

|Representação|
|:---------------------------:|
|Sistema Operacional          |
|⬆️                           |
|Job 1                        |
|⬆️                           |
|Job 2                        |
|⬆️                           |
|Job 3                        |
|⬆️                           |
|Job 4                        |
|⬆️                           |
|Job 5                        |



> 1. Dispositivo de entrada usado para ler cartões perfurados e enviar os dados ao computador por uma interface serial.
>
> 2. Processo de decidir a ordem, momento e quanto tempo cada job vai usar os recursos do sistema.