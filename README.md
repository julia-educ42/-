import React, { useState } from 'react';
import { ChevronRight, ChevronLeft, Check, BookOpen, Users, MessageSquare, Target, Award } from 'lucide-react';

const SupervisorCourse = () => {
  const [currentScreen, setCurrentScreen] = useState('welcome');
  const [currentModule, setCurrentModule] = useState(0);
  const [currentLesson, setCurrentLesson] = useState(0);
  const [completedLessons, setCompletedLessons] = useState([]);
  const [quizAnswers, setQuizAnswers] = useState({});

  const modules = [
    {
      id: 1,
      title: "Роль супервизора",
      icon: Users,
      color: "#9987F5",
      lessons: [
        {
          title: "Что делает супервизор?",
          content: "Супервизор — это связующее звено между командой и руководством. Ваша главная задача — помочь команде работать эффективно и расти профессионально.",
          keyPoints: [
            "Координация работы команды",
            "Решение операционных вопросов",
            "Поддержка и развитие сотрудников",
            "Контроль качества работы"
          ]
        },
        {
          title: "Переход от оператора к супервизору",
          content: "Это нормально чувствовать неуверенность. Вы уже знаете работу изнутри — это ваше преимущество. Теперь учитесь смотреть шире.",
          keyPoints: [
            "Фокус смещается с задач на людей",
            "Вы больше не делаете всё сами",
            "Ваш успех = успех команды",
            "Учитесь делегировать"
          ]
        }
      ]
    },
    {
      id: 2,
      title: "Коммуникация",
      icon: MessageSquare,
      color: "#2E7F53",
      lessons: [
        {
          title: "Как давать обратную связь",
          content: "Обратная связь помогает людям расти. Главное правило: будьте конкретны и говорите о фактах, а не о личности.",
          keyPoints: [
            "Хвалите публично, критикуйте наедине",
            "Используйте формулу: факт + эффект + решение",
            "Слушайте ответ сотрудника",
            "Договоритесь о следующих шагах"
          ],
          example: "Вместо 'Ты работаешь плохо' → 'Вчера в трёх тикетах были ошибки. Это замедлило работу отдела. Давай разберём, что можно улучшить?'"
        },
        {
          title: "Сложные разговоры",
          content: "Конфликты и проблемы — нормальная часть работы. Ваша задача — решать их быстро и справедливо.",
          keyPoints: [
            "Не откладывайте разговор",
            "Подготовьтесь: факты, документы, решения",
            "Сохраняйте спокойствие",
            "Фокусируйтесь на решении"
          ]
        }
      ]
    },
    {
      id: 3,
      title: "Управление задачами",
      icon: Target,
      color: "#F5793B",
      lessons: [
        {
          title: "Приоритизация",
          content: "Не всё срочное — важное. Учитесь отличать и распределять задачи правильно.",
          keyPoints: [
            "Матрица Эйзенхауэра: срочное/важное",
            "Делегируйте рутину",
            "Оставляйте время на непредвиденное",
            "Защищайте команду от хаоса"
          ]
        },
        {
          title: "Планирование смен",
          content: "Эффективное планирование снижает стресс команды и улучшает качество сервиса.",
          keyPoints: [
            "Анализируйте нагрузку по часам/дням",
            "Учитывайте сильные стороны людей",
            "Ротация задач для развития",
            "Резерв на форс-мажоры"
          ]
        }
      ]
    },
    {
      id: 4,
      title: "Развитие команды",
      icon: Award,
      color: "#F296BD",
      lessons: [
        {
          title: "Онбординг новичков",
          content: "Первые недели определяют, останется ли человек и как быстро он начнёт приносить результат.",
          keyPoints: [
            "Назначьте наставника (бадди)",
            "Чёткий план на первые 30 дней",
            "Регулярные встречи для вопросов",
            "Отмечайте первые успехи"
          ]
        },
        {
          title: "Мотивация команды",
          content: "Деньги мотивируют, но не только они. Люди хотят чувствовать смысл и признание.",
          keyPoints: [
            "Благодарите за конкретные действия",
            "Давайте интересные задачи",
            "Показывайте карьерные перспективы",
            "Создавайте командный дух"
          ]
        }
      ]
    }
  ];

  const completeLesson = () => {
    const lessonId = `${currentModule}-${currentLesson}`;
    if (!completedLessons.includes(lessonId)) {
      setCompletedLessons([...completedLessons, lessonId]);
    }
  };

  const goToNextLesson = () => {
    completeLesson();
    const module = modules[currentModule];
    if (currentLesson < module.lessons.length - 1) {
      setCurrentLesson(currentLesson + 1);
    } else if (currentModule < modules.length - 1) {
      setCurrentModule(currentModule + 1);
      setCurrentLesson(0);
    } else {
      setCurrentScreen('completion');
    }
  };

  const goToPrevLesson = () => {
    if (currentLesson > 0) {
      setCurrentLesson(currentLesson - 1);
    } else if (currentModule > 0) {
      setCurrentModule(currentModule - 1);
      setCurrentLesson(modules[currentModule - 1].lessons.length - 1);
    }
  };

  const getProgress = () => {
    const totalLessons = modules.reduce((sum, m) => sum + m.lessons.length, 0);
    return Math.round((completedLessons.length / totalLessons) * 100);
  };

  if (currentScreen === 'welcome') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-purple-50 p-4 md:p-8">
        <div className="max-w-4xl mx-auto">
          <div className="bg-white rounded-3xl shadow-xl p-8 md:p-12">
            <div className="flex items-center justify-center mb-8">
              <div className="bg-gradient-to-r from-purple-400 to-blue-400 rounded-full p-4">
                <BookOpen className="w-12 h-12 text-white" />
              </div>
            </div>
            
            <h1 className="text-4xl md:text-5xl font-bold text-center mb-4 text-gray-800">
              Основы управления<br />командой саппорта
            </h1>
            
            <p className="text-xl text-center text-gray-600 mb-8">
              Практический курс для новых супервизоров
            </p>
            
            <div className="grid md:grid-cols-2 gap-6 mb-10">
              <div className="bg-blue-50 rounded-2xl p-6">
                <h3 className="font-semibold text-lg mb-3 text-gray-800">Что вы узнаете</h3>
                <ul className="space-y-2 text-gray-700">
                  <li className="flex items-start">
                    <span className="text-blue-500 mr-2">✓</span>
                    <span>Как организовать работу команды</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-blue-500 mr-2">✓</span>
                    <span>Эффективная коммуникация</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-blue-500 mr-2">✓</span>
                    <span>Управление задачами и приоритетами</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-blue-500 mr-2">✓</span>
                    <span>Развитие и мотивация сотрудников</span>
                  </li>
                </ul>
              </div>
              
              <div className="bg-purple-50 rounded-2xl p-6">
                <h3 className="font-semibold text-lg mb-3 text-gray-800">Формат обучения</h3>
                <ul className="space-y-2 text-gray-700">
                  <li className="flex items-start">
                    <span className="text-purple-500 mr-2">◆</span>
                    <span>4 модуля с практическими уроками</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-purple-500 mr-2">◆</span>
                    <span>Реальные примеры из практики</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-purple-500 mr-2">◆</span>
                    <span>Около 45 минут обучения</span>
                  </li>
                  <li className="flex items-start">
                    <span className="text-purple-500 mr-2">◆</span>
                    <span>Учитесь в своём темпе</span>
                  </li>
                </ul>
              </div>
            </div>
            
            <button
              onClick={() => setCurrentScreen('modules')}
              className="w-full bg-gradient-to-r from-purple-500 to-blue-500 text-white py-4 rounded-2xl font-semibold text-lg hover:shadow-lg transition-all duration-300 flex items-center justify-center gap-2"
            >
              Начать обучение
              <ChevronRight className="w-6 h-6" />
            </button>
          </div>
        </div>
      </div>
    );
  }

  if (currentScreen === 'modules') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-purple-50 p-4 md:p-8">
        <div className="max-w-5xl mx-auto">
          <div className="bg-white rounded-3xl shadow-xl p-8 md:p-12">
            <h1 className="text-3xl md:text-4xl font-bold text-center mb-4 text-gray-800">
              Программа курса
            </h1>
            
            <div className="mb-8">
              <div className="flex justify-between items-center mb-2">
                <span className="text-sm text-gray-600">Ваш прогресс</span>
                <span className="text-sm font-semibold text-gray-800">{getProgress()}%</span>
              </div>
              <div className="w-full bg-gray-200 rounded-full h-3">
                <div 
                  className="bg-gradient-to-r from-purple-500 to-blue-500 h-3 rounded-full transition-all duration-500"
                  style={{ width: `${getProgress()}%` }}
                />
              </div>
            </div>
            
            <div className="grid md:grid-cols-2 gap-6">
              {modules.map((module, idx) => {
                const Icon = module.icon;
                const moduleCompleted = module.lessons.every((_, lessonIdx) => 
                  completedLessons.includes(`${idx}-${lessonIdx}`)
                );
                
                return (
                  <div
                    key={module.id}
                    onClick={() => {
                      setCurrentModule(idx);
                      setCurrentLesson(0);
                      setCurrentScreen('lesson');
                    }}
                    className="bg-gradient-to-br from-white to-gray-50 rounded-2xl p-6 border-2 border-gray-100 hover:border-gray-300 cursor-pointer transition-all duration-300 hover:shadow-lg relative overflow-hidden group"
                  >
                    <div 
                      className="absolute top-0 left-0 w-2 h-full transition-all duration-300 group-hover:w-3"
                      style={{ backgroundColor: module.color }}
                    />
                    
                    <div className="flex items-start gap-4 ml-4">
                      <div 
                        className="rounded-xl p-3 flex-shrink-0"
                        style={{ backgroundColor: `${module.color}20` }}
                      >
                        <Icon className="w-6 h-6" style={{ color: module.color }} />
                      </div>
                      
                      <div className="flex-1">
                        <div className="flex items-center justify-between mb-2">
                          <h3 className="font-bold text-lg text-gray-800">{module.title}</h3>
                          {moduleCompleted && (
                            <div className="bg-green-100 rounded-full p-1">
                              <Check className="w-4 h-4 text-green-600" />
                            </div>
                          )}
                        </div>
                        
                        <p className="text-sm text-gray-600 mb-3">
                          {module.lessons.length} {module.lessons.length === 1 ? 'урок' : 'урока'}
                        </p>
                        
                        <div className="space-y-1">
                          {module.lessons.map((lesson, lessonIdx) => {
                            const isCompleted = completedLessons.includes(`${idx}-${lessonIdx}`);
                            return (
                              <div key={lessonIdx} className="flex items-center gap-2 text-sm">
                                <div className={`w-2 h-2 rounded-full ${isCompleted ? 'bg-green-500' : 'bg-gray-300'}`} />
                                <span className={isCompleted ? 'text-gray-800' : 'text-gray-500'}>
                                  {lesson.title}
                                </span>
                              </div>
                            );
                          })}
                        </div>
                      </div>
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        </div>
      </div>
    );
  }

  if (currentScreen === 'lesson') {
    const module = modules[currentModule];
    const lesson = module.lessons[currentLesson];
    const Icon = module.icon;
    const isCompleted = completedLessons.includes(`${currentModule}-${currentLesson}`);
    const isLastLesson = currentModule === modules.length - 1 && 
                         currentLesson === module.lessons.length - 1;

    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-purple-50 p-4 md:p-8">
        <div className="max-w-4xl mx-auto">
          <div className="bg-white rounded-3xl shadow-xl overflow-hidden">
            <div 
              className="p-6 md:p-8"
              style={{ backgroundColor: `${module.color}10` }}
            >
              <button
                onClick={() => setCurrentScreen('modules')}
                className="flex items-center gap-2 text-gray-600 hover:text-gray-800 mb-4 transition-colors"
              >
                <ChevronLeft className="w-5 h-5" />
                <span>К модулям</span>
              </button>
              
              <div className="flex items-center gap-3 mb-2">
                <div 
                  className="rounded-xl p-2"
                  style={{ backgroundColor: `${module.color}30` }}
                >
                  <Icon className="w-5 h-5" style={{ color: module.color }} />
                </div>
                <span className="text-sm font-medium text-gray-600">
                  Модуль {currentModule + 1}: {module.title}
                </span>
              </div>
              
              <h1 className="text-3xl md:text-4xl font-bold text-gray-800 mb-4">
                {lesson.title}
              </h1>
              
              <div className="flex items-center gap-2 text-sm text-gray-600">
                <span>Урок {currentLesson + 1} из {module.lessons.length}</span>
                {isCompleted && (
                  <>
                    <span>•</span>
                    <span className="flex items-center gap-1 text-green-600">
                      <Check className="w-4 h-4" />
                      Пройдено
                    </span>
                  </>
                )}
              </div>
            </div>
            
            <div className="p-6 md:p-8">
              <div className="prose max-w-none mb-8">
                <p className="text-lg text-gray-700 leading-relaxed mb-6">
                  {lesson.content}
                </p>
                
                <div className="bg-gradient-to-br from-blue-50 to-purple-50 rounded-2xl p-6 mb-6">
                  <h3 className="font-bold text-lg mb-4 text-gray-800">Ключевые моменты:</h3>
                  <ul className="space-y-3">
                    {lesson.keyPoints.map((point, idx) => (
                      <li key={idx} className="flex items-start gap-3">
                        <div 
                          className="rounded-full p-1 mt-0.5 flex-shrink-0"
                          style={{ backgroundColor: module.color }}
                        >
                          <Check className="w-4 h-4 text-white" />
                        </div>
                        <span className="text-gray-700">{point}</span>
                      </li>
                    ))}
                  </ul>
                </div>
                
                {lesson.example && (
                  <div className="bg-yellow-50 border-l-4 border-yellow-400 rounded-r-2xl p-6">
                    <h4 className="font-bold text-gray-800 mb-2 flex items-center gap-2">
                      <span className="text-2xl">💡</span>
                      Пример из практики
                    </h4>
                    <p className="text-gray-700">{lesson.example}</p>
                  </div>
                )}
              </div>
              
              <div className="flex gap-4">
                {(currentModule > 0 || currentLesson > 0) && (
                  <button
                    onClick={goToPrevLesson}
                    className="flex-1 border-2 border-gray-300 text-gray-700 py-3 rounded-xl font-semibold hover:bg-gray-50 transition-all duration-300 flex items-center justify-center gap-2"
                  >
                    <ChevronLeft className="w-5 h-5" />
                    Назад
                  </button>
                )}
                
                <button
                  onClick={goToNextLesson}
                  className="flex-1 text-white py-3 rounded-xl font-semibold hover:shadow-lg transition-all duration-300 flex items-center justify-center gap-2"
                  style={{ 
                    background: `linear-gradient(135deg, ${module.color}, ${module.color}dd)` 
                  }}
                >
                  {isLastLesson ? 'Завершить курс' : 'Далее'}
                  <ChevronRight className="w-5 h-5" />
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }

  if (currentScreen === 'completion') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-purple-50 p-4 md:p-8 flex items-center justify-center">
        <div className="max-w-2xl mx-auto">
          <div className="bg-white rounded-3xl shadow-xl p-8 md:p-12 text-center">
            <div className="flex justify-center mb-6">
              <div className="bg-gradient-to-r from-green-400 to-emerald-400 rounded-full p-6">
                <Award className="w-16 h-16 text-white" />
              </div>
            </div>
            
            <h1 className="text-4xl md:text-5xl font-bold mb-4 text-gray-800">
              Поздравляем!
            </h1>
            
            <p className="text-xl text-gray-600 mb-8">
              Вы успешно завершили курс по основам управления командой саппорта
            </p>
            
            <div className="bg-gradient-to-br from-blue-50 to-purple-50 rounded-2xl p-6 mb-8">
              <h3 className="font-bold text-lg mb-4 text-gray-800">Что дальше?</h3>
              <ul className="text-left space-y-3 text-gray-700">
                <li className="flex items-start gap-3">
                  <span className="text-green-500 text-xl">✓</span>
                  <span>Применяйте полученные знания в ежедневной работе</span>
                </li>
                <li className="flex items-start gap-3">
                  <span className="text-green-500 text-xl">✓</span>
                  <span>Возвращайтесь к материалам курса при необходимости</span>
                </li>
                <li className="flex items-start gap-3">
                  <span className="text-green-500 text-xl">✓</span>
                  <span>Обсудите новые подходы с вашим руководителем</span>
                </li>
                <li className="flex items-start gap-3">
                  <span className="text-green-500 text-xl">✓</span>
                  <span>Продолжайте учиться и развивать свои навыки</span>
                </li>
              </ul>
            </div>
            
            <button
              onClick={() => {
                setCurrentScreen('modules');
              }}
              className="w-full bg-gradient-to-r from-purple-500 to-blue-500 text-white py-4 rounded-2xl font-semibold text-lg hover:shadow-lg transition-all duration-300 flex items-center justify-center gap-2"
            >
              Вернуться к модулям
            </button>
          </div>
        </div>
      </div>
    );
  }
};

export default SupervisorCourse;
