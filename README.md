## Simples aplicação de exemplo usando o Socket.io

Recentemente comecei a focar os meus estudos no protocolo de WebSocket.
Por conta disso, realizei a criação desse projeto apenas para fixar os meus conhecimentos.

## Segue imagens do código e um video de exemplo.

# implementação do Socket.io

import express from "express";
import socketio from "socket.io";
import http from "http";
import path from "path";

const app = express();
const httpServer = http.createServer(app);
const io = new socketio.Server(httpServer);

app.use(express.static(path.resolve(__dirname, "..", "public")));

io.on("connection", (socket) => {
  console.log(`New connection: ${socket.id}`);

  socket.on("message", (message) => {
    console.log(message);

    socket.emit("received", `Received Message ${message}`);
  });
});

httpServer.listen(3333);

# Consumo do client web

 let socket;

      document.getElementById("join").addEventListener("click", function () {
        if (!socket) {
          socket = new io("http://localhost:3333");
        }

        socket.on(`received`, (message) => {
          console.log(message);
        });
      });

      document
        .getElementById("sendmessage")
        .addEventListener("click", function () {
          if (!socket) {
            return;
          }

          socket.emit("message", `Hellou world ${Math.random()}`);
        });
