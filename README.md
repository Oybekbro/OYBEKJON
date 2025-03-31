# OYBEKJON
there is all mything
// Murakkab Next.js asosli Admin Panel + AI Chatbot + Kontent Tahlili + Tavsiyalar + Avtomatik Tasdiqlash + Xavfsizlik Monitoringi + Avtomatik Yangilanish + AI UI Generator + AI Xavfsizlik + Avtomatik Ruxsat + Shaxsiy Kabinet + AI Panel + Telegram/Instagram/YouTube Integratsiyasi + Foydalanuvchilar uchun shaxsiy chat + Media almashish + Profil sozlamalari // Next.js + Tailwind CSS + ShadCN UI + NextAuth + Prisma + PostgreSQL + OpenAI API + Puppeteer + TensorFlow.js + Google Search API + Yandex Search API + Cloudflare DDoS + Anti-Phishing + Telegram Bot API + Instagram API + YouTube API + WebSockets // Apple.com uslubidagi premium UI dizayn

import { useState, useEffect } from "react"; import { useSession } from "next-auth/react"; import { useRouter } from "next/router"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Switch } from "@/components/ui/switch"; import { Select, SelectItem } from "@/components/ui/select"; import { FileUpload } from "@/components/ui/file-upload"; import { motion } from "framer-motion"; import axios from "axios"; import io from "socket.io-client";

const socket = io("https://yourserver.com");

export default function SecureApp() { const { data: session } = useSession(); const router = useRouter(); const [query, setQuery] = useState(""); const [results, setResults] = useState([]); const [userIP, setUserIP] = useState(null); const [darkMode, setDarkMode] = useState(false); const [theme, setTheme] = useState("default"); const [uploadedFiles, setUploadedFiles] = useState([]); const [uploadedVideos, setUploadedVideos] = useState([]); const [aiResponse, setAIResponse] = useState(""); const [messages, setMessages] = useState([]); const [message, setMessage] = useState(""); const [profilePic, setProfilePic] = useState(null); const [birthday, setBirthday] = useState(""); const [bio, setBio] = useState("");

useEffect(() => { socket.on("receive_message", (data) => { setMessages((prev) => [...prev, data]); }); }, []);

const sendMessage = () => { if (message.trim()) { const msgData = { user: session?.user?.email, text: message }; socket.emit("send_message", msgData); setMessages((prev) => [...prev, msgData]); setMessage(""); } };

return ( <div className={p-6 ${darkMode ? "bg-gray-900 text-white" : "bg-white text-black"}}> <motion.h1 className="text-3xl font-bold mb-4" animate={{ opacity: 1, scale: 1.1 }} transition={{ duration: 0.5 }}>Shaxsiy Kabinet</motion.h1>

<div className="mb-6">
    <h2 className="text-xl font-bold mb-2">Profil Sozlamalari</h2>
    <div className="flex flex-col gap-4">
      <FileUpload onUpload={(files) => setProfilePic(files[0])} allowedTypes={["image"]} />
      {profilePic && <img src={profilePic.url} alt="Profil rasmi" className="w-24 h-24 rounded-full" />}
      <Input type="date" value={birthday} onChange={(e) => setBirthday(e.target.value)} placeholder="Tug'ilgan kun" />
      <Input type="text" value={bio} onChange={(e) => setBio(e.target.value)} placeholder="Qisqacha bio" />
    </div>
  </div>

  <div className="mb-6">
    <h2 className="text-xl font-bold mb-2">Shaxsiy Chat</h2>
    <div className="border p-4 rounded-lg h-64 overflow-y-auto">
      {messages.map((msg, index) => (
        <div key={index} className="mb-2">
          <span className="font-bold">{msg.user}:</span> {msg.text}
        </div>
      ))}
    </div>
    <div className="mt-2 flex gap-2">
      <Input value={message} onChange={(e) => setMessage(e.target.value)} placeholder="Xabar yozing..." />
      <Button onClick={sendMessage}>Yuborish</Button>
    </div>
  </div>
  
  <div className="mt-6">
    <h2 className="text-xl font-bold mb-2">Fayl yuklash</h2>
    <FileUpload onUpload={(files) => setUploadedFiles([...uploadedFiles, ...files])} allowedTypes={["image", "video"]} />
<div className="mt-4">
      {uploadedFiles.map((file, index) => (
        <div key={index} className="flex items-center gap-4 mb-2">
          <a href={file.url} target="_blank" rel="noopener noreferrer" className="text-blue-600">{file.name}</a>
          <span className="text-sm">{file.owner === session?.user?.email ? "Sizning faylingiz" : "Admin tomonidan boshqariladi"}</span>
        </div>
      ))}
    </div>
  </div>

  <div className="mt-6">
    <h2 className="text-xl font-bold mb-2">Video yuklash</h2>
    <FileUpload onUpload={(files) => setUploadedVideos([...uploadedVideos, ...files])} allowedTypes={["video"]} />
    <div className="mt-4">
      {uploadedVideos.map((video, index) => (
        <div key={index} className="mb-2">
          <video controls className="w-full rounded-lg">
            <source src={video.url} type="video/mp4" />
            Sizning brauzeringiz ushbu videoni qo‘llab-quvvatlamaydi.
          </video>
        </div>
      ))}
    </div>
  </div>
</div>

); }
