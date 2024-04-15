<script setup lang="ts">
import emitter from 'tiny-emitter/instance'
</script>

<template>
    <h3>Bluetooth</h3>
    <button @click="pair">Pair</button>
    <br>
    <textarea id="btMessages" cols="33" rows="11" v-model="messageText"></textarea>
</template>

<style scoped>
h3 {
    margin-top: 2em;
}

#btMessages {
    margin-top: 1em;
}
</style>

<script lang="ts">
/// <reference types="web-bluetooth" />

const UART_SERVICE = "6e400001-b5a3-f393-e0a9-e50e24dcca9e"
// Messages from PC to Device -- Controller Out Peripheral In
const UART_PICO_CHARACTERISTIC = "6e400002-b5a3-f393-e0a9-e50e24dcca9e"
// Messages from Device to PC -- Peripheral Out Controller In
const UART_POCI_CHARACTERISTIC = "6e400003-b5a3-f393-e0a9-e50e24dcca9e"

export default {
    data() {
        return {
            messageArray: Array(),
            messageText: "Bluetooth Messages Here"
        }
    },
    methods: {
        async pair() {
            try {
                console.log('Requesting Bluetooth Device...');
                const device = await navigator.bluetooth.requestDevice({
                    acceptAllDevices: true,
                    optionalServices: [UART_SERVICE]
                });

                console.log('Connecting to GATT Server...');
                const server = await device.gatt?.connect();

                console.log('Getting Service...');
                const service = await server?.getPrimaryService(UART_SERVICE);

                console.log('Getting Characteristic...');
                const myCharacteristic = await service?.getCharacteristic(UART_POCI_CHARACTERISTIC);

                console.log(myCharacteristic);

                await myCharacteristic?.startNotifications();

                console.log('> Notifications started');
                myCharacteristic?.addEventListener('characteristicvaluechanged', this.handleNotifications);
            } catch (error) {
                console.log('Problem: ' + error);
            }
        },
        handleNotifications(event: any) {
            let value = event.target.value;
            const decoder = new TextDecoder("utf-8")
            const text = decoder.decode(value)
            console.log(text)
            this.messageArray.push(text)
            if (this.messageArray.length > 10) {
                this.messageArray.shift()
            }
            this.messageText = this.messageArray.join("\n")
            emitter.emit('BTMessage', text)
        }
    }
}
</script>