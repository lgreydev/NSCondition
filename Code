import Foundation
import Darwin

var available = false
var condition = pthread_cond_t()
var mutex = pthread_mutex_t()

class MutexPrinter: Thread {

    override init() {
        pthread_cond_init(&condition, nil)
        pthread_mutex_init(&mutex, nil)
    }

    override func main() {
        printerMethod()

    }

    private func printerMethod() {
        pthread_mutex_lock(&mutex)
        print("Printer enter")
        while !available {
            pthread_cond_wait(&condition, &mutex)
        }
        available = false
        defer {
            pthread_mutex_unlock(&mutex)
            print(#function, "Printer")
        }
        print("Printer exit")
    }
}

class MutexWriter: Thread {

    override init() {
        pthread_cond_init(&condition, nil)
        pthread_mutex_init(&mutex, nil)
    }

    override func main() {
        writerMethod()

    }

    private func writerMethod() {
        pthread_mutex_lock(&mutex)
        print("Writer enter")
        available = true
        pthread_cond_signal(&condition)
        defer {
            pthread_mutex_unlock(&mutex)
            print(#function, "Writer")
        }
        print("Writer exit")
    }
}

let mutexPrinter = MutexPrinter()
let mutexWriter = MutexWriter()

mutexPrinter.start()
mutexWriter.start()
