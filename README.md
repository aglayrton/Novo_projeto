//1 momento
import React, { useEffect } from "react";
import axios from "axios";
import md5 from "md5";

//CONFIGURAÇÕES
//da onde vem a requisicao
const baseUrl = "http://gateway.marvel.com/v1/public/comics?";
//nossas chaves de acesso
const publicKey = "43f3fc2f913f61eec09ebd12354eca66";
const privateKey = "3d5b647ba4b01c978cf4fb4d26d0d21c2e6b1b94";
//para trabalhar o tempo
const time = Number(new Date()); //vamos precisar
//nossa hash
const hash = md5(time + privateKey + publicKey);

function App() {
useEffect(() => {
axios
.get(`${baseUrl}ts=${time}&apikey=${publicKey}&hash=${hash}`)
.then((response) => console.log(response.data))
.catch((error) => console.log(error));
}, []);

return (

<div className='App'>
<header className='App-header'></header>
</div>
);
}

# 2 momento separe tudo

import axios from "axios";
import md5 from "md5";

//CONFIGURAÇÕES
//da onde vem a requisicao
const baseUrl = "http://gateway.marvel.com/v1/public/comics?";
//nossas chaves de acesso
const publicKey = "43f3fc2f913f61eec09ebd12354eca66";
const privateKey = "3d5b647ba4b01c978cf4fb4d26d0d21c2e6b1b94";
//para trabalhar o tempo
const time = Number(new Date()); //vamos precisar
//nossa hash
const hash = md5(time + privateKey + publicKey);

import React from "react";

export const Personangens: React.FC = () => {
/_ useEffect(() => {
axios
.get(`${baseUrl}ts=${time}&apikey=${publicKey}&hash=${hash}`)
.then((response) => console.log(response.data))
.catch((error) => console.log(error));
}, []);
_/
return <h1>caracteres</h1>;
};

import React, { useEffect } from "react";

import { Personangens } from "./pages/Personagens";

function App() {
return (

<div className='App'>
<Personangens />
</div>
);
}

export default App;

# 3 momento

//CONFIGURAÇÕES
//da onde vem a requisicao
const baseUrl = "http://gateway.marvel.com/v1/public/";

import React, { useEffect, useState } from "react";
import api from "../../services/api";

export const Personangens: React.FC = () => {
const [personagens, setPersonagens] = useState([]);

useEffect(() => {
api
.get("/comics")
.then((response) => {
setPersonagens(response.data.data);
console.log(response.data.data);
})
.catch((error) => console.log(error));
}, []);

return <h1>Personagens</h1>;
};

# 4

import axios from "axios";
import md5 from "md5";

//CONFIGURAÇÕES
//da onde vem a requisicao
const baseUrl = "http://gateway.marvel.com/v1/public/";
//nossas chaves de acesso
const publicKey = "43f3fc2f913f61eec09ebd12354eca66";
const privateKey = "3d5b647ba4b01c978cf4fb4d26d0d21c2e6b1b94";
//para trabalhar o tempo
const time = Number(new Date()); //vamos precisar
//nossa hash
const hash = md5(time + privateKey + publicKey);

const api = axios.create({
baseURL: baseUrl,
params: {
ts: time,
apikey: publicKey,
hash,
},
});

export default api;

import React, { useEffect, useState } from "react";
import api from "../../services/api";

//temos que criar o tipo que vai ser trabalhado
interface ResponseData {
id: string;
name: string;
description: string;
thumbnail: {
path: string;
extension: string;
};
}

export const Personangens: React.FC = () => {
const [personagens, setPersonagens] = useState<ResponseData[]>([]);

useEffect(() => {
api
.get("/comics?")
.then((response) => {
setPersonagens(response.data.data.results);
console.log(response.data);
})
.catch((error) => console.log(error));
}, []);

return (
<>

<ul>
{personagens.map((personagem) => {
return (
<li>
<img
src={`${personagem.thumbnail.path}.${personagem.thumbnail.extension}`}
alt={`${personagem.name}`}
/>
</li>
);
})}
</ul>
</>
);
};

# 5

import React, { useEffect, useState } from "react";
import api from "../../services/api";

//temos que criar o tipo que vai ser trabalhado
interface ResponseData {
id: string;
title: string;
description: string;
thumbnail: {
path: string;
extension: string;
};
}

export const Personangens: React.FC = () => {
const [personagens, setPersonagens] = useState<ResponseData[]>([]);

useEffect(() => {
api
.get("/comics?")
.then((response) => {
setPersonagens(response.data.data.results);
console.log(response.data);
})
.catch((error) => console.log(error));
}, []);

return (
<>

<ul>
{personagens.map((personagem) => {
return (
<li key={personagem.id}>
<img
src={`${personagem.thumbnail.path}.${personagem.thumbnail.extension}`}
alt={`${personagem.title}`}
/>
<br></br>
<span>{`${personagem.title}`}</span>
<p>{personagem.description}</p>
</li>
);
})}
</ul>
</>
);
};

# 6
