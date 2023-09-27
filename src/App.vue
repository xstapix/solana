<script setup>
  import { reactive } from "vue";

  import { Metaplex, keypairIdentity, bundlrStorage, toMetaplexFileFromBrowser } from "@metaplex-foundation/js";
  import { Connection, Keypair } from "@solana/web3.js";

  const QUICKNODE_RPC = 'https://dark-autumn-moon.solana-devnet.discover.quiknode.pro/cb18b589f4dc195f3f281d80b3adb841a8af65cf/'
  const connection = new Connection(QUICKNODE_RPC); //конект к сети

  const secret = [66,227,215,70,88,217,253,89,137,223,14,131,249,116,151,46,0,105,135,255,87,49,172,140,175,37,139,136,117,181,250,136,221,249,237,147,144,128,12,58,56,42,85,39,176,215,55,20,215,66,101,115,180,18,145,133,54,46,168,75,207,205,244,19]; //секретный ключ
  const fromWallet = Keypair.fromSecretKey(new Uint8Array(secret)); //делаем тестовый публичный ключ

  const fileImg = reactive({
    data: null
  })

  let metaplex = Metaplex.make(connection)
    .use(keypairIdentity(fromWallet))
    .use(bundlrStorage({
      address: "https://devnet.bundlr.network",
      providerUrl: QUICKNODE_RPC,
      timeout: 60000,
    }),
  ); // инициализируем metaplex

  const createNFT = async () => {
    const file = await toMetaplexFileFromBrowser(fileImg.data)
    console.log(file);

    const imageUri = await metaplex.storage().upload(file); // тут ошибка
    console.log(imageUri);
  }

  const uploadImg = (file) => {
    fileImg.data = file.target.files[0]
  }
  
</script>

<template>
  <input type="file" name="" id="" @change="uploadImg">
  <button @click="createNFT()">CreateNFT</button>
</template>
