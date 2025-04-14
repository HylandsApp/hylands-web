<template>

    <!-- terminal container -->
    <div ref="terminalContainer" class="terminal-container"></div>

</template>

<style lang="scss">

    // make the terminal container fill the viewport
    .terminal-container {
        height: 100%;
    }

</style>

<script setup lang="ts">

    // imports
    import { onMounted, onBeforeUnmount, ref } from 'vue'
    import { Terminal } from 'xterm'
    import { FitAddon } from 'xterm-addon-fit'
    import 'xterm/css/xterm.css'

    // references
    const terminalContainer = ref<HTMLElement | null>(null)

    // variables
    let terminal: Terminal | null = null
    let fitAddon: FitAddon | null = null
    let ws: WebSocket | null = null
    let inputBuffer = ''
    let commandHistory: string[] = []
    let historyIndex = -1
    let currentInput = ''

    // initialize the terminal
    const initializeTerminal = () => {

        // check if the terminal container is available
        if (!terminalContainer.value) return
        
        // create a new terminal instance
        terminal = new Terminal({
            // rows: 40, // optional: override default size if needed
            scrollback: 1000,
            fontFamily: 'ui-monospace, SFMono-Regular, SF Mono, Menlo, Consolas, Liberation Mono, monospace',
            // fontFamily: 'PixelifySans',
            // fontFamily: 'Adventurer',
            // fontFamily: 'OpenDyslexicMono',
            // lineHeight: 0,
            // letterSpacing: 0,
            fontSize: 16,
            fontWeight: 400,
            fontWeightBold: 700,
            allowTransparency: true,
            // convertEol: true,
            cursorStyle: 'bar',
            cursorInactiveStyle: 'bar',
            cursorWidth: 3,
            cursorBlink: true,
            drawBoldTextInBrightColors: true,
            // logLevel: 1,
            minimumContrastRatio: 1,
            // screenReaderMode: true,
            // tabStopWidth: 0,
            theme: {
                background: '#00000000',
                cursor: '#ffffff',
                // cursorAccent: '#0000ff',
                foreground: '#ffffff',
                selectionBackground: '#00ff00',
                selectionForeground: '#000000',
                // selectionInactiveBackground: '#ff0000',
            }
        })
        
        // create and load the fit addon for responsive sizing.
        fitAddon = new FitAddon()
        terminal.loadAddon(fitAddon)
        
        // open the terminal in the container element.
        terminal.open(terminalContainer.value)

        // make the terminal fill the container.
        fitAddon.fit()

    }

    // connect to the websocket
    const connectWebSocket = () => {
        
        // change URL as appropriate. using ws:// for non-secure or wss:// if over HTTPS.
        ws = new WebSocket('ws://localhost:4001')

        // when the websocket connection opens, you might display a message.
        ws.onopen = () => {
            terminal?.write('\r\nConnected to Hylands!\r\n')
        }

        // write data received from the mud server to the terminal.
        ws.onmessage = (event) => {
            try {
                const data = JSON.parse(event.data)
                if (data.type === 'message' && data.message) {
                    terminal?.write('\r\n' + data.message + '\r\n')
                }
            } catch (e) {
                // if it's not JSON, write it directly
                terminal?.write('\r\n' + event.data + '\r\n')
            }
        }

        // on connection close, display a message.
        ws.onclose = () => {
            terminal?.write('\r\nConnection closed.')
        }

        // on connection error, display error details.
        ws.onerror = (error) => {
            terminal?.write('\r\nWebSocket error occurred.')
            console.error('WebSocket error:', error)
        }

        // when the user types in the terminal, send the data over the websocket.
        terminal?.onData((data) => {
            if (data === '\r' || data === '\n') {
                if (ws?.readyState === WebSocket.OPEN) {
                    ws.send(inputBuffer + '\r\n')
                    if (inputBuffer.trim()) {
                        commandHistory.push(inputBuffer)
                        historyIndex = commandHistory.length
                    }
                    inputBuffer = ''
                }
            } else if (data.charCodeAt(0) === 127) { // backspace
                if (inputBuffer.length > 0) {
                    inputBuffer = inputBuffer.slice(0, -1)
                    terminal?.write('\b \b')
                }
            } else if (data === '\x1b[A') { // up arrow
                if (historyIndex > 0) {
                    historyIndex--
                    if (historyIndex === commandHistory.length - 1) {
                        currentInput = inputBuffer
                    }
                    inputBuffer = commandHistory[historyIndex]
                    terminal?.write('\r\x1b[K' + inputBuffer)
                }
            } else if (data === '\x1b[B') { // down arrow
                if (historyIndex < commandHistory.length - 1) {
                    historyIndex++
                    inputBuffer = commandHistory[historyIndex]
                    terminal?.write('\r\x1b[K' + inputBuffer)
                } else if (historyIndex === commandHistory.length - 1) {
                    historyIndex++
                    inputBuffer = currentInput
                    terminal?.write('\r\x1b[K' + inputBuffer)
                }
            } else {
                inputBuffer += data
                terminal?.write(data)
            }
        })

    }

    onMounted(() => {

        // initialize terminal once component is mounted.
        initializeTerminal()
        connectWebSocket()

        // optionally, add a window resize listener to re-fit the terminal.
        window.addEventListener('resize', () => fitAddon?.fit())

    })

    // Clean up on unmounting the component
    onBeforeUnmount(() => {

        // remove the window resize listener
        window.removeEventListener('resize', () => fitAddon?.fit())

        // close the websocket
        if (ws) {
            ws.close()
        }

        // dispose of the terminal
        if (terminal) {
            terminal.dispose()
        }

    })

</script>