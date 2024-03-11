<script lang="ts" setup>
import { ElMessage } from 'element-plus'
import { Peer } from 'peerjs'
const ROOM_ID = 'room_name'

let peer: Peer

const createRoom = () => {
  peer = new Peer(ROOM_ID)
  peer.on('open', (id) => {
    console.log('创建成功', id)
  })
  peer.on('connection', (conn) => {
    console.log('有人加入', conn)
    conn.send('hello')
  })
  peer.on('disconnected', (id) => {
    console.log('断开连接', id)
  })
}

const joinRoom = () => {
  if (peer) {
    ElMessage.error('房间已经创建')
    return
  }
  peer = new Peer()
  peer.on('open', function (id) {
    console.log('加入房间, 自己的id' + id)
    const connection = peer.connect(ROOM_ID)

    connection.on('open', function () {
      console.log('已连接房间', ROOM_ID)
      // Handle data connection
    })
  })
}

const getUserMediaStream = () => {
  return navigator.mediaDevices.getUserMedia({ video: true, audio: true })
}
</script>

<template>
  <div class="border-base p-4">
    <code>Room ID: {{ ROOM_ID }}</code>
    <el-button type="primary" @click="createRoom">创建房间</el-button>
    <el-button @click="joinRoom">加入房间</el-button>
  </div>
</template>

<style lang="scss" scoped>
h2 {
  @apply bg-fuchsia-600;
}
</style>
