## 서버 코드
import java.net.ServerSocket
import java.net.Socket
import kotlin.concurrent.thread

val clients = mutableListOf<Socket>()

// 클라이언트 연결을 처리하는 함수
fun handleClient(clientSocket: Socket) {
    val clientAddress = clientSocket.inetAddress.hostAddress
    println("새로운 클라이언트 연결: $clientAddress")

    try {
        val inputStream = clientSocket.getInputStream()
        val outputStream = clientSocket.getOutputStream()

        while (true) {
            val buffer = ByteArray(1024)
            val bytesRead = inputStream.read(buffer)
            if (bytesRead == -1) break  // 연결이 끊어졌으면 종료

            val message = String(buffer, 0, bytesRead, Charsets.UTF_8)
            println("받은 메시지: $message")

            // 연결된 모든 클라이언트에게 메시지 전송
            for (client in clients) {
                if (client != clientSocket) {
                    client.getOutputStream().write(message.toByteArray(Charsets.UTF_8))
                }
            }
        }
    } catch (e: Exception) {
        println("오류 발생: ${e.message}")
    } finally {
        clients.remove(clientSocket)
        clientSocket.close()
    }
}

// 서버 시작 함수
fun startServer() {
    val serverSocket = ServerSocket(5555)
    println("서버가 시작되었습니다. 클라이언트를 기다립니다...")

    while (true) {
        val clientSocket = serverSocket.accept()
        clients.add(clientSocket)

        // 각 클라이언트에 대해 별도의 스레드로 처리
        thread { handleClient(clientSocket) }
    }
}

fun main() {
    startServer()
}


## 클라이언트 코드
import javafx.application.Application
import javafx.application.Platform
import javafx.scene.Scene
import javafx.scene.control.Button
import javafx.scene.control.TextArea
import javafx.scene.layout.VBox
import javafx.stage.Stage
import java.net.Socket
import kotlin.concurrent.thread

class ClientApp : Application() {
    private lateinit var clientSocket: Socket
    private lateinit var textArea: TextArea

    // 서버 IP와 포트
    private val serverIp = "서버_IP_주소"
    private val serverPort = 5555

    override fun start(primaryStage: Stage) {
        textArea = TextArea()
        textArea.isEditable = false

        val sendButton = Button("메시지 보내기")
        sendButton.setOnAction {
            sendMessage()
        }

        val root = VBox(10.0, textArea, sendButton)
        val scene = Scene(root, 400.0, 300.0)

        primaryStage.title = "커플 앱 연결"
        primaryStage.scene = scene
        primaryStage.show()

        // 서버와 연결
        startClient()
    }

    // 서버로 메시지를 보내는 함수
    private fun sendMessage() {
        val message = textArea.text.trim()
        if (message.isNotEmpty()) {
            clientSocket.getOutputStream().write(message.toByteArray(Charsets.UTF_8))
            textArea.clear()  // 메시지 전송 후 텍스트박스 초기화
        }
    }

    // 서버로부터 받은 메시지를 화면에 출력하는 함수
    private fun receiveMessage() {
        val inputStream = clientSocket.getInputStream()
        while (true) {
            try {
                val buffer = ByteArray(1024)
                val bytesRead = inputStream.read(buffer)
                if (bytesRead == -1) break  // 연결이 끊어졌으면 종료

                val message = String(buffer, 0, bytesRead, Charsets.UTF_8)
                Platform.runLater {
                    textArea.appendText("상대방: $message\n")
                }
            } catch (e: Exception) {
                println("오류 발생: ${e.message}")
                break
            }
        }
    }

    // 서버와 연결하는 함수
    private fun startClient() {
        clientSocket = Socket(serverIp, serverPort)

        // 메시지 수신을 위한 스레드
        thread {
            receiveMessage()
        }
    }
}

fun main() {
    Application.launch(ClientApp::class.java)
}
