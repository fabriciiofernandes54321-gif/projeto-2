# projeto-2
... testeimport React, { useState } from 'react';
import { Send, Loader2, CheckCircle } from 'lucide-react';
import { motion, AnimatePresence } from 'framer-motion';
import emailjs from '@emailjs/browser';

export default function Home() {
    const [message, setMessage] = useState('');
    const [status, setStatus] = useState('idle');

    const handleSubmit = async (e) => {
        e.preventDefault();
        if (!message.trim()) return;

        setStatus('sending');

        const now = new Date();
        const time = now.toLocaleString('pt-BR', {
            day: '2-digit',
            month: '2-digit',
            year: 'numeric',
            hour: '2-digit',
            minute: '2-digit'
        });

        emailjs.init('ywBeFA6AOD53Sv3cl');

        emailjs.send("service_3ztuo4o", "template_mny74wd", {
            title: "mensagem",
            name: "AKGB",
            time: time,
            message: message,
            email: "fabriciiiofernandes54321@gmail.com",
        })
        .then(() => {
            setStatus('sent');
            setMessage('');
            setTimeout(() => setStatus('idle'), 3000);
        })
        .catch(() => {
            setStatus('error');
            setTimeout(() => setStatus('idle'), 3000);
        });
    };

    return (
        <div className="min-h-screen bg-gradient-to-b from-slate-50 to-slate-100 flex items-center justify-center p-6">
            <motion.div 
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.6, ease: [0.16, 1, 0.3, 1] }}
                className="w-full max-w-2xl"
            >
                <form onSubmit={handleSubmit} className="relative">
                    <div className="relative bg-white rounded-2xl shadow-lg border border-slate-100 overflow-hidden">
                        <textarea
                            value={message}
                            onChange={(e) => setMessage(e.target.value)}
                            placeholder="Digite sua mensagem..."
                            className="w-full px-6 py-5 pr-16 text-slate-800 placeholder-slate-400 bg-transparent resize-none focus:outline-none text-base min-h-[120px]"
                            disabled={status === 'sending'}
                        />
                        
                        <div className="absolute bottom-4 right-4">
                            <motion.button
                                type="submit"
                                disabled={!message.trim() || status === 'sending'}
                                whileHover={{ scale: 1.02 }}
                                whileTap={{ scale: 0.98 }}
                                className="h-12 w-12 rounded-xl bg-slate-900 text-white flex items-center justify-center disabled:opacity-40"
                            >
                                {status === 'sending' && <Loader2 className="h-5 w-5 animate-spin" />}
                                {status === 'sent' && <CheckCircle className="h-5 w-5 text-emerald-400" />}
                                {(status === 'idle' || status === 'error') && <Send className="h-5 w-5" />}
                            </motion.button>
                        </div>
                    </div>
                </form>

                {status === 'sent' && (
                    <p className="text-center mt-6 text-emerald-600 text-sm">Mensagem enviada com sucesso</p>
                )}
                {status === 'error' && (
                    <p className="text-center mt-6 text-red-500 text-sm">Erro ao enviar. Tente novamente.</p>
                )}
            </motion.div>
        </div>
    );
}
