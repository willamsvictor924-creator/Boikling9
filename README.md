{
  "name": "boikling9",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.22.0",
    "react-dropzone": "^14.2.3",
    "framer-motion": "^10.16.4",
    "tailwindcss": "^3.4.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  }
}boikling9/
│
├── public/
│   ├── index.html
│   └── manifest.json
│
├── src/
│   ├── App.js
│   ├── index.js
│   ├── components/
│   │   ├── Header.js
│   │   ├── Login.js
│   │   ├── UploadBook.js
│   │   ├── Translator.js
│   │   ├── Library.js
│   │   └── Plans.js
│   ├── config/
│   │   ├── languages.js
│   │   └── payments.js
│   └── styles.css
│
└── package.json<!DOCTYPE html>
<html lang="pt">
  <head>
    <meta charset="UTF-8" />
    <meta name="theme-color" content="#111" />
    <title>Boikling9</title>
  </head>
  <body class="bg-gray-900 text-white">
    <div id="root"></div>
  </body>
</html>import React from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Header from "./components/Header";
import Login from "./components/Login";
import UploadBook from "./components/UploadBook";
import Translator from "./components/Translator";
import Library from "./components/Library";
import Plans from "./components/Plans";

export default function App() {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="/" element={<Login />} />
        <Route path="/upload" element={<UploadBook />} />
        <Route path="/translate" element={<Translator />} />
        <Route path="/library" element={<Library />} />
        <Route path="/plans" element={<Plans />} />
      </Routes>
    </BrowserRouter>
  );
}import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./styles.css";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);import React from "react";
import { Link } from "react-router-dom";

export default function Header() {
  return (
    <header className="bg-gray-800 p-4 flex justify-between">
      <h1 className="font-bold text-xl">Boikling9</h1>
      <nav className="flex gap-4">
        <Link to="/upload">Upload</Link>
        <Link to="/library">Biblioteca</Link>
        <Link to="/plans">Planos</Link>
      </nav>
    </header>
  );
}import React, { useCallback } from "react";
import { useNavigate } from "react-router-dom";
import { useDropzone } from "react-dropzone";

export default function UploadBook() {
  const navigate = useNavigate();

  const onDrop = useCallback((files) => {
    const file = files[0];
    localStorage.setItem("uploadedBook", file.name);
    navigate("/translate");
  }, [navigate]);

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  return (
    <div {...getRootProps()} className="border-dashed border-2 border-gray-600 p-10 text-center cursor-pointer">
      <input {...getInputProps()} />
      <p>📚 Solte ou selecione seu e-book</p>
    </div>
  );
}import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { LANGUAGES } from "../config/languages";

export default function Login() {
  const [lang, setLang] = useState("pt");
  const navigate = useNavigate();

  const handleLogin = () => {
    localStorage.setItem("userLang", lang);
    navigate("/upload");
  };

  return (
    <div className="p-6 text-center">
      <h2 className="text-2xl mb-4">Escolha sua língua</h2>
      <select
        className="bg-gray-700 p-2 rounded"
        onChange={(e) => setLang(e.target.value)}
        value={lang}
      >
        {Object.entries(LANGUAGES).map(([code, name]) => (
          <option key={code} value={code}>{name}</option>
        ))}
      </select>
      <button
        className="mt-4 bg-blue-600 px-4 py-2 rounded"
        onClick={handleLogin}
      >
        Entrar
      </button>
    </div>
  );
}import React, { useState } from "react";

export default function Translator() {
  const [text, setText] = useState("");
  const [translated, setTranslated] = useState("");

  const translate = () => {
    setTranslated("🔁 Tradução simulada para o idioma selecionado!");
  };

  return (
    <div className="p-6 text-center">
      <textarea
        placeholder="Cole o texto do livro..."
        className="w-full h-40 bg-gray-800 p-2 rounded"
        onChange={(e) => setText(e.target.value)}
      />
      <button className="bg-green-600 px-4 py-2 rounded mt-2" onClick={translate}>
        Traduzir
      </button>
      <div className="bg-gray-700 mt-4 p-4 rounded">{translated}</div>
    </div>
  );
}import React from "react";

export default function Plans() {
  const plans = [
    { name: "Semanal", price: "R$ 24,90" },
    { name: "Bimestral", price: "R$ 54,90" },
    { name: "Anual", price: "R$ 231,50" }
  ];

  return (
    <div className="p-6 text-center">
      <h2 className="text-2xl mb-4">💳 Planos Boikling9</h2>
      <div className="flex flex-col gap-4">
        {plans.map((p) => (
          <div key={p.name} className="bg-gray-800 p-4 rounded">
            <h3>{p.name}</h3>
            <p>{p.price}</p>
          </div>
        ))}
      </div>
    </div>
  );
}import React from "react";

export default function Library() {
  const book = localStorage.getItem("uploadedBook");
  return (
    <div className="p-6 text-center">
      <h2 className="text-2xl mb-2">📚 Sua Biblioteca</h2>
      {book ? <p>{book}</p> : <p>Nenhum livro salvo ainda.</p>}
    </div>
  );
}export const LANGUAGES = {
  pt: "Português",
  en: "Inglês",
  es: "Espanhol",
  fr: "Francês",
  de: "Alemão",
  it: "Italiano",
  ja: "Japonês",
  zh: "Chinês"
};body {
  font-family: Arial, sans-serif;
}
a {
  color: #3b82f6;
}
button:hover {
  opacity: 0.8;
}export const PAYMENT_CONFIG = {
  stripeKey: "COLOQUE_SUA_CHAVE_STRIPE_AQUI", 719.930.184-76 
  mercadoPagoKey: "COLOQUE_SUA_CHAVE_MERCADOPAGO_AQUI",
  pixOwner: "willamsvictor924@gmail.com"
};
