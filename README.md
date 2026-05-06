import React, { useState, useEffect } from 'react';

// Имитация базы данных заданий
const INITIAL_TASKS = [
  {
    id: 1,
    title: "Доставка: Чистая Кухня",
    description: "Собрать посуду, загрузить посудомойку, протереть стол. Фото-отчет обязателен.",
    reward: 120,
    client: "Диспетчер: Мама",
    type: "Express",
    timeLeft: "2h"
  },
  {
    id: 2,
    title: "Контракт: Академический успех",
    description: "Получить средний балл выше 4.5 за месяц. Огромный бонус за дисциплину.",
    reward: 2500,
    client: "Заказчик: Папа",
    type: "Epic",
    timeLeft: "24d"
  },
  {
    id: 3,
    title: "Курьер: Закупка продуктов",
    description: "Сходить в магазин по списку. Вес пакетов до 5кг.",
    reward: 200,
    client: "Диспетчер: Мама",
    type: "Standard",
    timeLeft: "3h"
  }
];

export default function App() {
  const [tokens, setTokens] = useState(750);
  const [rating, setRating] = useState(4.92);
  const [tasks, setTasks] = useState(INITIAL_TASKS);

  const completeTask = (id, reward) => {
    setTokens(prev => prev + reward);
    // Повышаем рейтинг за успешное выполнение
    setRating(prev => Math.min(5.00, +(prev + 0.02).toFixed(2)));
    setTasks(prev => prev.filter(task => task.id !== id));
  };

  return (
    <div className="min-h-screen bg-[#F2F2F2] font-sans text-slate-900">
      {/* Header в стиле Glovo */}
      <header className="bg-[#FFC244] px-6 pt-12 pb-8 rounded-b-[40px] shadow-lg sticky top-0 z-10">
        <div className="flex justify-between items-start">
          <div>
            <h1 className="text-3xl font-black italic tracking-tighter text-slate-900">FAMILY COURIER</h1>
            <p className="text-sm font-bold opacity-70 mt-1 uppercase">Личный кабинет исполнителя</p>
          </div>
          <div className="bg-white/30 backdrop-blur-md rounded-2xl p-3 text-right">
            <span className="block text-[10px] font-bold opacity-60 uppercase">Рейтинг</span>
            <span className="text-xl font-black">★ {rating}</span>
          </div>
        </div>

        <div className="mt-8 bg-white rounded-3xl p-6 shadow-sm flex justify-between items-center">
          <div>
            <p className="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Доступный баланс</p>
            <p className="text-4xl font-black text-slate-900">{tokens.toLocaleString()} <span className="text-lg font-medium text-[#00A082]">₮</span></p>
          </div>
          <button className="bg-[#00A082] text-white px-6 py-3 rounded-2xl font-bold text-sm active:scale-95 transition-all">
            Вывести
          </button>
        </div>
      </header>

      {/* Список заказов */}
      <main className="px-6 py-8">
        <div className="flex justify-between items-center mb-6">
          <h2 className="text-xl font-black uppercase italic">Активные заказы</h2>
          <span className="bg-slate-200 text-slate-600 text-[10px] font-bold px-3 py-1 rounded-full">
            {tasks.length} ДОСТУПНО
          </span>
        </div>

        <div className="space-y-4">
          {tasks.map(task => (
            <div key={task.id} className="bg-white rounded-[28px] p-5 shadow-sm border border-transparent hover:border-[#00A082] transition-all group">
              <div className="flex justify-between items-start mb-2">
                <span className={`text-[10px] font-black px-3 py-1 rounded-full uppercase ${
                  task.type === 'Epic' ? 'bg-purple-100 text-purple-600' : 'bg-green-100 text-green-600'
                }`}>
                  {task.type}
                </span>
                <span className="text-xs font-bold text-slate-400 italic">Таймер: {task.timeLeft}</span>
              </div>

              <h3 className="text-lg font-bold leading-tight mb-2 group-hover:text-[#00A082] transition-colors">{task.title}</h3>
              <p className="text-sm text-slate-500 mb-6 line-clamp-2">{task.description}</p>

              <div className="flex items-center justify-between pt-4 border-t border-slate-50">
                <div className="flex flex-col">
                  <span className="text-[10px] font-bold text-slate-400 uppercase tracking-tighter italic">{task.client}</span>
                  <span className="text-xl font-black text-slate-900">+{task.reward} ₮</span>
                </div>
                <button 
                  onClick={() => completeTask(task.id, task.reward)}
                  className="bg-[#FFC244] text-slate-900 font-extrabold px-6 py-3 rounded-2xl text-sm shadow-md active:translate-y-1 transition-all"
                >
                  ПРИНЯТЬ
                </button>
              </div>
            </div>
          ))}

          {tasks.length === 0 && (
            <div className="text-center py-20">
              <div className="text-5xl mb-4">📦</div>
              <p className="font-bold text-slate-400">Все заказы доставлены!<br/>Диспетчер скоро назначит новые.</p>
            </div>
          )}
        </div>
      </main>
    </div>
  );
}
