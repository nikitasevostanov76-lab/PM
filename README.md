<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>PM Learning Tracker</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #141416; color: #f0f0f0; min-height: 100vh; }

:root {
  --accent: #1D9E75; --accent-light: #0a3326; --accent-mid: #5DCAA5; --accent-dark: #5DCAA5;
  --amber: #f0a030; --amber-light: #2a1a00;
  --blue: #5b9cf0; --blue-light: #0a1f3a;
  --coral: #e87050; --coral-light: #2a0e05;
  --purple: #9b93f0; --purple-light: #1a1640;
  --border: rgba(255,255,255,0.09); --border-strong: rgba(255,255,255,0.18);
  --bg: #1e1e22; --bg2: #141416; --bg3: #2a2a2e;
  --text: #f0f0f0; --text2: #aaa; --text3: #666;
  --radius: 10px; --radius-lg: 14px;
}
}

.header { background: var(--bg); border-bottom: 0.5px solid var(--border); padding: 16px 20px 0; position: sticky; top: 0; z-index: 100; }
.header-top { display: flex; align-items: center; gap: 10px; margin-bottom: 14px; }
.logo { width: 32px; height: 32px; background: var(--accent); border-radius: 8px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.logo svg { width: 18px; height: 18px; }
.header-title { font-size: 17px; font-weight: 600; color: var(--text); }
.header-sub { font-size: 12px; color: var(--text3); }

.tabs { display: flex; gap: 0; }
.tab { flex: 1; padding: 10px 0; font-size: 14px; font-weight: 500; cursor: pointer; border: none; background: none; border-bottom: 2px solid transparent; color: var(--text2); transition: all 0.15s; }
.tab.active { color: var(--accent); border-bottom-color: var(--accent); }

.content { padding: 20px 16px; max-width: 800px; margin: 0 auto; }

/* STATS */
.stats-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 24px; }
.stat-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); padding: 14px 12px; }
.stat-label { font-size: 11px; color: var(--text2); margin-bottom: 4px; }
.stat-value { font-size: 24px; font-weight: 600; color: var(--text); line-height: 1; }
.stat-sub { font-size: 11px; color: var(--text3); margin-top: 3px; }
.pbar { height: 5px; background: var(--bg3); border-radius: 99px; margin-top: 8px; overflow: hidden; }
.pbar-fill { height: 100%; background: var(--accent); border-radius: 99px; transition: width 0.4s; }

/* SECTION */
.section-label { font-size: 11px; font-weight: 600; color: var(--text3); text-transform: uppercase; letter-spacing: 0.07em; margin: 20px 0 10px; }

/* TOPIC CARDS */
.topic-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 14px 14px; margin-bottom: 8px; display: flex; align-items: center; gap: 12px; cursor: pointer; -webkit-tap-highlight-color: transparent; transition: border-color 0.12s; }
.topic-card:active { background: var(--bg3); }
.topic-card.done { background: var(--bg2); }

.check { width: 24px; height: 24px; border-radius: 7px; border: 1.5px solid var(--border-strong); display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: all 0.15s; }
.topic-card.done .check { background: var(--accent); border-color: var(--accent); }
.check-icon { display: none; }
.topic-card.done .check-icon { display: block; }

.topic-body { flex: 1; min-width: 0; }
.topic-name { font-size: 14px; font-weight: 500; color: var(--text); line-height: 1.3; }
.topic-card.done .topic-name { color: var(--text2); text-decoration: line-through; text-decoration-color: var(--text3); }
.topic-meta { font-size: 12px; color: var(--text3); margin-top: 2px; }

.badge { font-size: 10px; font-weight: 600; padding: 3px 7px; border-radius: 6px; white-space: nowrap; flex-shrink: 0; }
.b-theory { background: var(--blue-light); color: var(--blue); }
.b-practice { background: var(--amber-light); color: var(--amber); }
.b-tools { background: var(--purple-light); color: var(--purple); }
.b-process { background: var(--coral-light); color: var(--coral); }

.read-btn { font-size: 13px; color: var(--blue); background: none; border: none; cursor: pointer; padding: 4px 0; flex-shrink: 0; white-space: nowrap; }

/* LIBRARY */
.lib-layout { display: flex; gap: 0; min-height: calc(100vh - 140px); }

@media (min-width: 700px) {
  .lib-nav { width: 220px; flex-shrink: 0; border-right: 0.5px solid var(--border); padding-right: 16px; position: sticky; top: 100px; align-self: flex-start; max-height: calc(100vh - 120px); overflow-y: auto; }
  .lib-body { flex: 1; padding-left: 24px; min-width: 0; }
}
@media (max-width: 699px) {
  .lib-layout { flex-direction: column; }
  .lib-nav { display: flex; overflow-x: auto; gap: 6px; padding-bottom: 12px; border-bottom: 0.5px solid var(--border); margin-bottom: 16px; -webkit-overflow-scrolling: touch; scrollbar-width: none; }
  .lib-nav::-webkit-scrollbar { display: none; }
  .lib-nav-item { white-space: nowrap; border-radius: 20px; padding: 7px 14px; border: 0.5px solid var(--border); font-size: 13px; }
  .lib-nav-item.active { background: var(--accent-light); color: var(--accent-dark); border-color: var(--accent-mid); }
}

@media (min-width: 700px) {
  .lib-nav-item { display: flex; align-items: center; gap: 8px; padding: 9px 12px; border-radius: var(--radius); cursor: pointer; font-size: 13px; color: var(--text2); margin-bottom: 2px; border: none; background: none; width: 100%; text-align: left; transition: all 0.1s; }
  .lib-nav-item:hover { background: var(--bg3); color: var(--text); }
  .lib-nav-item.active { background: var(--accent-light); color: var(--accent-dark); font-weight: 500; }
  .nav-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; opacity: 0.6; }
}

.lib-article { display: none; }
.lib-article.active { display: block; }

.art-title { font-size: 22px; font-weight: 700; color: var(--text); margin-bottom: 6px; line-height: 1.2; }
.art-tag { display: inline-block; font-size: 11px; font-weight: 600; padding: 3px 8px; border-radius: 6px; margin-bottom: 14px; }
.art-intro { font-size: 15px; line-height: 1.75; color: var(--text); margin-bottom: 20px; }

.key-points { background: var(--bg3); border-radius: var(--radius-lg); padding: 16px 18px; margin-bottom: 20px; }
.kp-label { font-size: 11px; font-weight: 600; color: var(--text2); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 10px; }
.kp-row { display: flex; gap: 10px; align-items: flex-start; margin-bottom: 8px; font-size: 14px; line-height: 1.6; color: var(--text); }
.kp-row:last-child { margin-bottom: 0; }
.kp-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); flex-shrink: 0; margin-top: 7px; }

.phases { display: flex; gap: 5px; flex-wrap: wrap; margin: 12px 0; }
.phase { flex: 1; min-width: 70px; background: var(--blue-light); border-radius: var(--radius); padding: 10px 6px; text-align: center; }
.phase-n { font-size: 10px; color: var(--blue); font-weight: 600; }
.phase-name { font-size: 11px; font-weight: 500; color: var(--blue); margin-top: 2px; }

.sub-sec { margin-bottom: 18px; }
.sub-title { font-size: 15px; font-weight: 600; color: var(--text); margin-bottom: 7px; padding-left: 10px; border-left: 3px solid var(--accent-mid); }
.sub-text { font-size: 14px; line-height: 1.7; color: var(--text); }

.info-box { border-radius: var(--radius); padding: 12px 14px; margin: 14px 0; font-size: 13px; line-height: 1.6; border-left: 3px solid; }
.ib-tip { background: var(--accent-light); border-color: var(--accent); color: var(--accent-dark); }
.ib-warn { background: var(--amber-light); border-color: var(--amber); color: var(--amber); }
.ib-note { background: var(--blue-light); border-color: var(--blue); color: var(--blue); }

.terms-label { font-size: 11px; font-weight: 600; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin: 18px 0 10px; }
.terms-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
@media (max-width: 500px) { .terms-grid { grid-template-columns: 1fr; } }
.term { background: var(--bg2); border-radius: var(--radius); padding: 11px 13px; }
.term-word { font-size: 13px; font-weight: 600; color: var(--text); margin-bottom: 3px; }
.term-def { font-size: 12px; color: var(--text2); line-height: 1.5; }

.mark-btn { margin-top: 24px; padding: 13px 24px; background: var(--accent); color: #fff; border: none; border-radius: var(--radius); font-size: 14px; font-weight: 600; cursor: pointer; width: 100%; transition: opacity 0.15s; }
.mark-btn:active { opacity: 0.8; }
.mark-btn.done-state { background: var(--bg3); color: var(--text2); cursor: default; }

/* PANEL */
.panel { display: none; }
.panel.active { display: block; }
</style>
</head>
<body>

<div class="header">
  <div class="header-top">
    <div class="logo">
      <svg viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="2" y="2" width="6" height="6" rx="1.5" fill="white" opacity="0.9"/>
        <rect x="10" y="2" width="6" height="6" rx="1.5" fill="white" opacity="0.6"/>
        <rect x="2" y="10" width="6" height="6" rx="1.5" fill="white" opacity="0.6"/>
        <rect x="10" y="10" width="6" height="6" rx="1.5" fill="white" opacity="0.4"/>
      </svg>
    </div>
    <div>
      <div class="header-title">PM Learning Tracker</div>
      <div class="header-sub">Путь к Junior Project Manager</div>
    </div>
  </div>
  <div class="tabs">
    <button class="tab active" onclick="switchTab('tracker')">Трекер</button>
    <button class="tab" onclick="switchTab('library')">Библиотека</button>
  </div>
</div>

<div class="content">

  <!-- TRACKER -->
  <div class="panel active" id="tab-tracker">
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-label">Пройдено</div>
        <div class="stat-value" id="s-done">0</div>
        <div class="stat-sub">из 18 тем</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">Прогресс</div>
        <div class="stat-value" id="s-pct">0%</div>
        <div class="pbar"><div class="pbar-fill" id="s-bar" style="width:0%"></div></div>
      </div>
      <div class="stat-card">
        <div class="stat-label">Осталось</div>
        <div class="stat-value" id="s-left">18</div>
        <div class="stat-sub">до Junior PM</div>
      </div>
    </div>
    <div id="tracker-body"></div>
  </div>

  <!-- LIBRARY -->
  <div class="panel" id="tab-library">
    <div class="lib-layout">
      <div class="lib-nav" id="lib-nav"></div>
      <div class="lib-body" id="lib-body"></div>
    </div>
  </div>

</div>

<script>
const STORAGE_KEY = 'pm_done_v3';
const topics = [
  {id:'t1',g:1,name:'Что такое проект и проектный менеджмент',meta:'Базовые концепции',badge:'theory',art:'a1'},
  {id:'t2',g:1,name:'Жизненный цикл проекта',meta:'Фазы и этапы',badge:'process',art:'a2'},
  {id:'t3',g:1,name:'Ключевые роли: PM, стейкхолдеры, команда',meta:'Кто есть кто',badge:'theory',art:'a3'},
  {id:'t4',g:1,name:'Тройное ограничение: Scope / Time / Cost',meta:'Железный треугольник',badge:'theory',art:'a4'},
  {id:'t5',g:2,name:'Waterfall — каскадная методология',meta:'Классика управления',badge:'process',art:'a5'},
  {id:'t6',g:2,name:'Agile — манифест и принципы',meta:'Гибкие методологии',badge:'process',art:'a6'},
  {id:'t7',g:2,name:'Scrum — роли, события, артефакты',meta:'Самый популярный фреймворк',badge:'process',art:'a7'},
  {id:'t8',g:2,name:'Kanban — доска и поток работ',meta:'Визуальное управление',badge:'tools',art:'a8'},
  {id:'t9',g:3,name:'Устав проекта (Project Charter)',meta:'Официальный старт',badge:'process',art:'a9'},
  {id:'t10',g:3,name:'Scope и WBS — декомпозиция работ',meta:'Что входит в проект',badge:'process',art:'a10'},
  {id:'t11',g:3,name:'Оценка сроков и бюджета',meta:'Как считать время и деньги',badge:'practice',art:'a11'},
  {id:'t12',g:3,name:'Диаграмма Ганта и критический путь',meta:'Планирование графика',badge:'tools',art:'a12'},
  {id:'t13',g:3,name:'Управление рисками',meta:'Идентификация и реагирование',badge:'practice',art:'a13'},
  {id:'t14',g:3,name:'Популярные PM-инструменты',meta:'Jira, Notion, Asana, Trello',badge:'tools',art:'a14'},
  {id:'t15',g:4,name:'Управление стейкхолдерами',meta:'Заинтересованные стороны',badge:'practice',art:'a15'},
  {id:'t16',g:4,name:'Коммуникации в проекте',meta:'Планы и инструменты',badge:'practice',art:'a16'},
  {id:'t17',g:4,name:'Управление качеством',meta:'QA и контроль',badge:'theory',art:'a17'},
  {id:'t18',g:4,name:'Закрытие проекта и ретроспектива',meta:'Lessons learned',badge:'process',art:'a18'},
];

const groups = {1:'Основы и контекст',2:'Методологии',3:'Планирование и инструменты',4:'Команда и коммуникация'};
const badgeLabel = {theory:'Теория',practice:'Практика',tools:'Инструменты',process:'Процессы'};

const articles = {
  a1:{title:'Что такое проект и проектный менеджмент',tag:'Теория',badge:'theory',intro:'Проект — это временное усилие, предпринятое для создания уникального продукта, услуги или результата. В отличие от операционной деятельности, проект имеет чёткое начало и конец.',kp:['Проект ограничен по времени: у него есть дата старта и дата завершения','Результат проекта всегда уникален — это не рутинная повторяющаяся работа','Проект требует ресурсов: людей, денег, времени, оборудования','Проектный менеджмент — это применение знаний, навыков и инструментов для достижения целей проекта'],sections:[{t:'Чем проект отличается от операций',p:'Операционная деятельность — это повторяющиеся процессы (например, ежемесячный выпуск зарплат). Проект — создание новой CRM-системы для автоматизации этой зарплаты. После завершения проекта система становится частью операций.'},{t:'Зачем нужен PM?',p:'Без системного управления проекты часто выходят за рамки бюджета, срываются по срокам или не достигают заявленных целей. PM обеспечивает структуру, координацию и контроль.'}],info:{type:'tip',text:'По статистике PMI, организации с развитой культурой PM успешно завершают 89% проектов, против 36% у тех, кто не применяет PM-практики.'},terms:[{w:'Portfolio',d:'Совокупность программ и проектов для достижения стратегических целей организации'},{w:'Program',d:'Группа связанных проектов, управляемых совместно для получения преимуществ'},{w:'PMBOK',d:'Project Management Body of Knowledge — главный стандарт PMI'},{w:'PMO',d:'Project Management Office — отдел, который стандартизирует PM-практики'}]},
  a2:{title:'Жизненный цикл проекта',tag:'Процессы',badge:'process',intro:'Жизненный цикл проекта — это последовательность фаз, через которые проходит проект от инициации до закрытия. Понимание цикла помогает PM знать, что делать на каждом этапе.',kp:['Классический цикл состоит из 5 групп процессов по PMBOK','На ранних фазах стоимость изменений минимальна — чем позже, тем дороже','Каждая фаза завершается deliverable — конкретным результатом','Цикл может быть предиктивным (waterfall) или адаптивным (agile)'],phases:['Инициация','Планирование','Исполнение','Мониторинг','Закрытие'],sections:[{t:'Инициация',p:'Определяется бизнес-кейс, назначается PM, создаётся устав проекта. Главный вопрос: "Стоит ли вообще делать этот проект?"'},{t:'Планирование',p:'Самая объёмная группа процессов. Создаётся план управления проектом, определяются scope, schedule, budget, риски, коммуникации.'},{t:'Исполнение',p:'Команда выполняет работу по плану. PM координирует людей и ресурсы, управляет ожиданиями стейкхолдеров.'}],info:{type:'note',text:'Мониторинг и контроль — единственная группа, которая работает непрерывно во время всего проекта, а не только на одном этапе.'},terms:[{w:'Deliverable',d:'Измеримый результат или выход, который должен быть произведён для завершения проекта'},{w:'Milestone',d:'Значимая точка или событие в проекте — маркер достижения без самостоятельной работы'},{w:'Gate review',d:'Точка принятия решения между фазами: продолжать, изменить или остановить проект'},{w:'Baseline',d:'Утверждённый план (scope, time, cost), с которым сравниваются фактические показатели'}]},
  a3:{title:'Ключевые роли в проекте',tag:'Теория',badge:'theory',intro:'В каждом проекте есть ключевые участники с разными зонами ответственности. PM должен понимать, кто есть кто, и уметь работать с каждым из них.',kp:['PM (Project Manager) — центральная фигура, отвечает за достижение целей проекта','Спонсор проекта — выделяет ресурсы и принимает ключевые решения','Команда проекта — выполняет работу по созданию результатов','Стейкхолдеры — все, кто влияет на проект или испытывает его влияние'],sections:[{t:'Роль Project Manager',p:'PM планирует, организует, контролирует и завершает проект. Он не обязан быть техническим экспертом, но должен уметь координировать экспертов, управлять рисками и коммуникациями.'},{t:'Спонсор vs PM',p:'Спонсор (Project Sponsor) — заказчик внутри организации, который финансирует проект и несёт бизнес-ответственность. PM отчитывается перед спонсором и работает в его интересах.'},{t:'Матрица RACI',p:'Инструмент распределения ответственности: R (Responsible) — кто делает, A (Accountable) — кто отвечает, C (Consulted) — кого спрашивают, I (Informed) — кого информируют.'}],info:{type:'tip',text:'На интервью на Junior PM часто спрашивают: "Чем PM отличается от Team Lead?" PM управляет проектом (временная задача), Team Lead — командой (постоянная структура).'},terms:[{w:'RACI',d:'Матрица ответственности: Responsible, Accountable, Consulted, Informed'},{w:'Спонсор',d:'Лицо или группа, предоставляющие ресурсы и поддержку проекта'},{w:'Функциональный менеджер',d:'Руководитель отдела, который "одалживает" ресурсы проекту'},{w:'Стейкхолдер',d:'Любое лицо или организация, которые влияют на проект или находятся под его влиянием'}]},
  a4:{title:'Тройное ограничение: Scope / Time / Cost',tag:'Теория',badge:'theory',intro:'Железный треугольник — фундаментальная концепция PM. Каждый проект ограничен тремя параметрами: содержанием (что делаем), сроками (когда) и стоимостью (за сколько). Изменение одного влечёт изменение других.',kp:['Scope — полное описание работ и результатов проекта','Time — расписание, сроки выполнения задач и milestone-ов','Cost — бюджет: деньги, люди, оборудование','Качество — часто добавляют как четвёртый параметр (расширенный треугольник)'],sections:[{t:'Как работает треугольник',p:'Если клиент хочет добавить новые функции (+ Scope), то либо нужно больше времени (+Time), либо больше денег (+Cost), либо сократить что-то из исходного scope. Хорошая поговорка: "Быстро, дёшево, качественно — выбери любые два".'},{t:'Scope Creep',p:'Неконтролируемое расширение объёма работ — одна из главных причин провала проектов. PM должен управлять изменениями через формальный процесс Change Control.'}],info:{type:'warn',text:'Scope Creep убивает проекты. Если стейкхолдер говорит "давай просто добавим ещё одну маленькую фичу", PM должен оценить влияние на time и cost, и только потом давать ответ.'},terms:[{w:'Scope Creep',d:'Неконтролируемые изменения или непрерывный рост объёма работ без корректировки времени и бюджета'},{w:'Change Control',d:'Формальный процесс рассмотрения и одобрения изменений в проекте'},{w:'Trade-off',d:'Компромисс: намеренное принятие ухудшения одного параметра ради улучшения другого'},{w:'Quality',d:'Степень соответствия результатов проекта требованиям и ожиданиям'}]},
  a5:{title:'Waterfall — каскадная методология',tag:'Процессы',badge:'process',intro:'Waterfall (каскадная модель) — последовательный подход к разработке, где каждая фаза должна быть полностью завершена перед началом следующей. Предиктивный подход: всё планируется заранее.',kp:['Фазы идут строго последовательно, возврат назад крайне дорогостоящий','Хорошо работает, когда требования чёткие и стабильные с самого начала','Документация — ключевая часть каждой фазы','Результат виден только в конце, что создаёт риски'],phases:['Требования','Проектирование','Разработка','Тестирование','Развёртывание'],sections:[{t:'Когда применять Waterfall',p:'Строительство, производство, крупные инфраструктурные проекты, государственные контракты с фиксированными требованиями. Там, где изменения физически или юридически невозможны после начала работ.'},{t:'Минусы Waterfall',p:'Негибкость к изменениям, поздняя обратная связь от клиента, риск "водопада" неправильных решений от фазы к фазе. Если требования изменились на этапе тестирования — огромные потери.'}],info:{type:'note',text:'Waterfall не устарел — он применим там, где он применим. Ошибка — использовать его для проектов с нечёткими или часто меняющимися требованиями.'},terms:[{w:'Предиктивный подход',d:'Весь scope определяется в начале, планирование ведётся на весь проект сразу'},{w:'Adaptive approach',d:'Итеративный или инкрементальный подход, где scope уточняется по ходу проекта'},{w:'Requirements freeze',d:'Момент фиксации требований, после которого изменения проходят через формальный процесс'},{w:'Sign-off',d:'Формальное утверждение результатов фазы заказчиком перед переходом к следующей'}]},
  a6:{title:'Agile — манифест и принципы',tag:'Процессы',badge:'process',intro:'Agile — это набор ценностей и принципов для разработки программного обеспечения, сформулированных в Agile Manifesto (2001). Не методология, а философия гибкого подхода к работе.',kp:['4 ключевые ценности и 12 принципов Agile Manifesto','Фокус на людях и взаимодействии, а не процессах и инструментах','Работающий продукт важнее исчерпывающей документации','Реагирование на изменения важнее следования плану'],sections:[{t:'4 ценности Agile Manifesto',p:'1) Люди и взаимодействие важнее процессов и инструментов. 2) Работающий продукт важнее исчерпывающей документации. 3) Сотрудничество с заказчиком важнее согласования условий контракта. 4) Готовность к изменениям важнее следования первоначальному плану.'},{t:'Agile-фреймворки',p:'Agile — это не конкретная методология, а зонтичный термин. Под ним находятся конкретные фреймворки: Scrum, Kanban, XP (Extreme Programming), SAFe (Scaled Agile Framework), LeSS.'}],info:{type:'tip',text:'Ценности звучат как "X важнее Y", но это не значит, что Y не важен совсем. Документация нужна, просто работающий продукт — приоритет.'},terms:[{w:'Iteration',d:'Фиксированный промежуток времени для выполнения работы и получения обратной связи (Sprint в Scrum)'},{w:'Increment',d:'Рабочий фрагмент продукта, готовый к потенциальной поставке заказчику'},{w:'Velocity',d:'Количество работы (story points), которое команда выполняет за итерацию'},{w:'Definition of Done',d:'Общее соглашение команды о том, что значит "задача завершена"'}]},
  a7:{title:'Scrum — роли, события, артефакты',tag:'Процессы',badge:'process',intro:'Scrum — самый популярный Agile-фреймворк. Структурированный эмпирический процесс для разработки сложных продуктов через короткие итерации (Sprint) с постоянной обратной связью.',kp:['3 роли: Product Owner, Scrum Master, Development Team','5 событий: Sprint, Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective','3 артефакта: Product Backlog, Sprint Backlog, Increment','Sprint длится 1-4 недели, обычно 2 недели'],sections:[{t:'Product Owner (PO)',p:'Отвечает за максимизацию ценности продукта. Управляет Product Backlog: приоритизирует задачи, формулирует требования, принимает или отклоняет результаты Sprint.'},{t:'Scrum Master (SM)',p:'Сервисный лидер: помогает команде следовать принципам Scrum, убирает препятствия (impediments), организует Scrum-события. Не менеджер и не начальник.'},{t:'Development Team',p:'Самоорганизующаяся кросс-функциональная команда 3-9 человек. Сама решает, как выполнять работу. Несёт коллективную ответственность за Increment.'}],info:{type:'warn',text:'Junior PM часто путают: в Scrum нет роли "Project Manager". PM-функции распределены между PO (что делать) и SM (как организовать процесс).'},terms:[{w:'Sprint',d:'Фиксированная итерация (1-4 нед.), в конце которой создаётся потенциально поставляемый Increment'},{w:'Product Backlog',d:'Упорядоченный список всего, что может понадобиться в продукте — единственный источник требований'},{w:'User Story',d:'Формат задачи: "Как [роль] я хочу [действие], чтобы [ценность]"'},{w:'Story Points',d:'Относительная мера сложности задачи, используемая для оценки и расчёта velocity'}]},
  a8:{title:'Kanban — доска и поток работ',tag:'Инструменты',badge:'tools',intro:'Kanban — метод визуализации рабочего потока и управления работой в процессе (WIP). Родом из производственной системы Toyota. Гибкий, не предполагает итераций.',kp:['Визуализация всей работы на доске с колонками (To Do → In Progress → Done)','Ограничение WIP (Work In Progress) — ключевой принцип','Непрерывный поток вместо итераций: задачи берутся по мере готовности','Метрики: Lead Time, Cycle Time, Throughput'],sections:[{t:'WIP-лимиты',p:'Ограничение числа задач в колонке "В работе" — главная особенность Kanban. Если лимит достигнут, новую задачу нельзя взять, пока не завершена предыдущая. Это выявляет узкие места.'},{t:'Kanban vs Scrum',p:'Kanban лучше для операционных процессов с непредсказуемым потоком (техподдержка, bug fixes). Scrum — для разработки нового продукта с планируемыми итерациями.'}],info:{type:'tip',text:'Kanban-доска в Trello или Jira — отличный инструмент даже без строгого следования всем принципам Kanban. Многие Junior PM начинают с неё.'},terms:[{w:'WIP limit',d:'Максимальное число задач, которые могут одновременно находиться в одной колонке'},{w:'Lead Time',d:'Общее время от момента создания задачи до её завершения'},{w:'Cycle Time',d:'Время от момента начала работы над задачей до её завершения (без ожидания)'},{w:'Throughput',d:'Количество задач, завершённых за единицу времени'}]},
  a9:{title:'Устав проекта (Project Charter)',tag:'Процессы',badge:'process',intro:'Project Charter — официальный документ, который формально авторизует проект. Без него проект не существует с точки зрения организации. Один из самых важных документов для Junior PM.',kp:['Официально авторизует существование проекта','Назначает PM и даёт ему полномочия','Фиксирует цели, scope, ключевые риски и стейкхолдеров','Подписывается спонсором — именно его подпись даёт проекту жизнь'],sections:[{t:'Что входит в Charter',p:'Цели и обоснование проекта, измеримые показатели успеха, высокоуровневые требования, предположения и ограничения, высокоуровневые риски, сводное расписание и бюджет, список стейкхолдеров, имя PM и его полномочия.'},{t:'Business Case',p:'Обоснование необходимости проекта с финансовой точки зрения. Обычно создаётся до Charter и является его основой. Включает ROI, NPV, Payback Period.'}],info:{type:'note',text:'Charter — не детальный план. Это высокоуровневый документ. Детали появятся в плане управления проектом, который создаётся на фазе планирования.'},terms:[{w:'Business Case',d:'Документ, обосновывающий инвестиции в проект: проблема, решение, финансовая выгода'},{w:'ROI',d:'Return on Investment — возврат на инвестиции, ключевая метрика бизнес-кейса'},{w:'Assumption',d:'Предположение, которое принимается за истину при планировании, но требует проверки'},{w:'Constraint',d:'Ограничение, которое влияет на исполнение проекта (бюджет, дедлайн, законодательство)'}]},
  a10:{title:'Scope и WBS — декомпозиция работ',tag:'Процессы',badge:'process',intro:'Scope — полное описание всех работ и результатов проекта. WBS (Work Breakdown Structure) — иерархическая декомпозиция всего объёма работ на управляемые части.',kp:['Scope Statement — детальное описание что включено и что НЕ включено в проект','WBS разбивает проект до уровня work packages — конкретных задач','100% Rule: WBS должна охватывать 100% работ проекта','WBS Dictionary — описание каждого элемента WBS'],sections:[{t:'Как строить WBS',p:'Начни с финального результата проекта на верхнем уровне. Далее декомпозируй на major deliverables, затем на работы, затем на задачи (work packages). Правило: work package должен быть выполнен одним человеком или командой за определённое время.'},{t:'Product scope vs Project scope',p:'Product scope — характеристики продукта (что он умеет). Project scope — работы, необходимые для создания продукта. Оба scope должны быть чётко определены.'}],info:{type:'tip',text:'WBS — не список задач и не диаграмма Ганта. Это иерархическая структура результатов. Каждый узел дерева — это результат (noun), а не действие (verb).'},terms:[{w:'WBS',d:'Work Breakdown Structure — иерархическая декомпозиция всего объёма работ проекта'},{w:'Work Package',d:'Наименьший элемент WBS, которому можно назначить стоимость и исполнителя'},{w:'Deliverable',d:'Измеримый, проверяемый результат работы, который должен быть произведён'},{w:'Scope baseline',d:'Утверждённый scope statement + WBS + WBS Dictionary'}]},
  a11:{title:'Оценка сроков и бюджета',tag:'Практика',badge:'practice',intro:'Оценка — один из самых сложных навыков PM. Точная оценка сроков и бюджета критически важна для планирования проекта и управления ожиданиями стейкхолдеров.',kp:['Аналогичная оценка: на основе данных похожих прошлых проектов','Параметрическая: математическая модель (напр., 10 часов на страницу × 50 страниц)','Bottom-up: суммирование оценок всех work packages из WBS','Three-Point Estimate: оптимистичная + пессимистичная + наиболее вероятная'],sections:[{t:'PERT-формула',p:'Three-Point Estimate: E = (O + 4M + P) / 6, где O — оптимистичная, M — наиболее вероятная, P — пессимистичная. Даёт взвешенную оценку с учётом неопределённости.'},{t:'Contingency Reserve vs Management Reserve',p:'Contingency Reserve — буфер для известных рисков. Management Reserve — буфер для неизвестных рисков (на "чёрных лебедей"). Оба должны быть в бюджете.'}],info:{type:'warn',text:'Никогда не давай оценку "с потолка". Декомпозируй работу, оцени каждую часть, затем суммируй. И всегда добавляй буфер на непредвиденное.'},terms:[{w:'PERT',d:'Program Evaluation and Review Technique — метод оценки с тремя точками'},{w:'Contingency',d:'Резерв на покрытие известных рисков, включается в cost baseline'},{w:'Cost baseline',d:'Утверждённый бюджет с разбивкой по времени, без management reserve'},{w:'EVM',d:'Earned Value Management — система измерения исполнения проекта по стоимости и срокам'}]},
  a12:{title:'Диаграмма Ганта и критический путь',tag:'Инструменты',badge:'tools',intro:'Диаграмма Ганта — визуальное расписание проекта, показывающее задачи, их длительность и зависимости. Критический путь — самая длинная последовательность зависимых задач, определяющая минимальную длительность проекта.',kp:['Ганта: горизонтальные полосы показывают задачи и их временные рамки','Критический путь (CPM): любая задержка на нём откладывает завершение проекта','Float (slack) — запас времени у задачи без критического пути','Dependencies: FS, SS, FF, SF — четыре типа зависимостей между задачами'],sections:[{t:'Метод критического пути (CPM)',p:'Шаги: 1) Определи все задачи. 2) Оцени длительность. 3) Определи зависимости. 4) Рассчитай ранние и поздние старты/финиши. 5) Найди задачи с нулевым float — это и есть критический путь.'},{t:'Типы зависимостей',p:'Finish-to-Start (FS) — задача B начнётся только после завершения A (самый частый). Start-to-Start (SS) — B начнётся только после начала A. Finish-to-Finish (FF) — B завершится только после завершения A.'}],info:{type:'note',text:'Гантта умеют строить в MS Project, Asana, Monday.com, Jira. Но важнее понимать логику критического пути, чем умение кликать в программе.'},terms:[{w:'CPM',d:'Critical Path Method — метод нахождения самой длинной цепочки задач в проекте'},{w:'Float (Slack)',d:'Время, на которое можно задержать задачу без сдвига даты завершения проекта'},{w:'Lag',d:'Намеренная задержка между задачами'},{w:'Lead',d:'Перекрытие задач: B начинается до завершения A, чтобы сэкономить время'}]},
  a13:{title:'Управление рисками',tag:'Практика',badge:'practice',intro:'Риск — неопределённое событие или условие, которое при возникновении оказывает положительное или отрицательное влияние на проект. PM управляет рисками проактивно, а не реагирует на проблемы.',kp:['Риски бывают угрозами (негативные) и возможностями (позитивные)','Процесс: Идентификация → Качественный анализ → Количественный анализ → Планирование → Мониторинг','Матрица вероятность × воздействие — базовый инструмент оценки','Реестр рисков — живой документ, обновляемый на всём протяжении проекта'],sections:[{t:'Стратегии реагирования на угрозы',p:'1) Avoid — изменить план, чтобы риск не возник. 2) Transfer — переложить ответственность (страхование, субподряд). 3) Mitigate — снизить вероятность или воздействие. 4) Accept — принять риск.'},{t:'Стратегии для возможностей',p:'1) Exploit — сделать так, чтобы возможность точно реализовалась. 2) Share — разделить возможность с партнёром. 3) Enhance — увеличить вероятность или воздействие. 4) Accept.'}],info:{type:'tip',text:'Главное правило рисков: риск, о котором знают, — уже не риск, а известная неопределённость. Худшие риски те, которые не идентифицированы.'},terms:[{w:'Risk Register',d:'Документ с перечнем всех выявленных рисков, их оценкой и планами реагирования'},{w:'Risk Appetite',d:'Степень неопределённости, которую организация готова принять в погоне за ценностью'},{w:'Residual Risk',d:'Риск, остающийся после применения стратегий реагирования'},{w:'Trigger',d:'Условие или признак, сигнализирующий о том, что риск вот-вот реализуется'}]},
  a14:{title:'Популярные PM-инструменты',tag:'Инструменты',badge:'tools',intro:'Junior PM должен уметь работать хотя бы с одним-двумя инструментами из каждой категории. Работодатели часто указывают конкретные инструменты в вакансиях.',kp:['Jira — стандарт для Agile-команд и разработки ПО','Notion — документация, wiki, простые задачи','Asana / Monday.com — управление проектами и задачами','MS Project — профессиональное расписание и CPM (корпоративный сектор)'],sections:[{t:'Jira',p:'Продукт Atlassian, инструмент №1 для Agile/Scrum/Kanban-команд. Backlogs, спринты, эпики, пользовательские истории, отчёты. Умение работать в Jira — обязательно для Junior PM в tech-компаниях.'},{t:'Notion',p:'Гибкое пространство для документов, баз данных и задач. Отлично подходит для небольших команд, стартапов, ведения PM-документации: project charter, meeting notes, risk register.'}],info:{type:'tip',text:'Совет для портфолио: зарегистрируйся в Jira (бесплатно до 10 пользователей) и создай учебный проект. Скриншоты реального использования инструмента сильнее любых слов в резюме.'},terms:[{w:'Jira',d:'Система управления задачами и проектами от Atlassian, стандарт в IT'},{w:'Confluence',d:'Корпоративная wiki от Atlassian для хранения документации проекта'},{w:'Trello',d:'Простая Kanban-доска от Atlassian, идеальна для небольших проектов'},{w:'Notion',d:'Гибкий all-in-one инструмент: документы + задачи + базы данных'}]},
  a15:{title:'Управление стейкхолдерами',tag:'Практика',badge:'practice',intro:'Стейкхолдеры могут сделать или сломать проект. PM должен их идентифицировать, понять интересы и ожидания, и выстроить стратегию взаимодействия с каждым.',kp:['Stakeholder Register — список всех стейкхолдеров с их интересами и влиянием','Power/Interest Grid — матрица для приоритизации внимания к стейкхолдерам','Цель: перевести стейкхолдеров из "Resistant" в "Supportive"','Вовлечённость — непрерывный процесс, а не разовое действие'],sections:[{t:'Power/Interest Grid',p:'Матрица 2×2: по осям — власть и интерес. Стратегии: High Power/High Interest — Manage closely. High Power/Low Interest — Keep satisfied. Low Power/High Interest — Keep informed. Low Power/Low Interest — Monitor.'},{t:'Уровни вовлечённости',p:'Unaware → Resistant → Neutral → Supportive → Leading. PM должен знать текущий уровень каждого стейкхолдера и целевой уровень, и строить план для движения к нему.'}],info:{type:'warn',text:'Никогда не игнорируй "неудобных" стейкхолдеров. Несогласованный стейкхолдер с высокой властью может закрыть проект в любой момент.'},terms:[{w:'Stakeholder Register',d:'Документ с данными о стейкхолдерах: роль, интересы, влияние, текущая и желаемая вовлечённость'},{w:'Power/Interest Grid',d:'Инструмент приоритизации стейкхолдеров по оси власти и интереса к проекту'},{w:'Engagement Matrix',d:'Матрица текущего vs желаемого уровня вовлечённости каждого стейкхолдера'},{w:'Champion',d:'Стейкхолдер, активно поддерживающий проект и помогающий устранять препятствия'}]},
  a16:{title:'Коммуникации в проекте',tag:'Практика',badge:'practice',intro:'По статистике PM тратит 90% времени на коммуникации. Плохие коммуникации — вторая по частоте причина провала проектов. План коммуникаций отвечает на вопросы: кому, что, как, когда, кто.',kp:['Communication Management Plan — документ, описывающий все коммуникации проекта','Формула каналов: n(n-1)/2, где n — число участников','Pull vs Push communication: активная рассылка vs информация по запросу','Встречи, статус-отчёты, dashboard — ключевые инструменты PM'],sections:[{t:'Формула каналов коммуникации',p:'n(n-1)/2: при 5 участниках = 10 каналов, при 10 = 45 каналов. Это объясняет, почему большая команда требует больше усилий на координацию — сложность растёт экспоненциально.'},{t:'Эффективные встречи',p:'У каждой встречи должна быть чёткая цель. Правила: agenda заранее, фиксированное время начала и конца, протокол с action items, ответственными и дедлайнами.'}],info:{type:'tip',text:'Правило PM: "Communicate early, communicate often." Лучше перекоммуницировать, чем оставить стейкхолдера в неведении.'},terms:[{w:'Status Report',d:'Регулярный отчёт о ходе проекта: что сделано, что планируется, риски, проблемы'},{w:'Escalation',d:'Передача проблемы на более высокий уровень управления для принятия решения'},{w:'Action Item',d:'Конкретная задача с ответственным и дедлайном, возникшая по итогам встречи'},{w:'Dashboard',d:'Визуальный отчёт о ключевых показателях проекта в реальном времени'}]},
  a17:{title:'Управление качеством',tag:'Теория',badge:'theory',intro:'Качество в PM — степень соответствия результатов проекта задокументированным требованиям и пригодность для использования. PM не обеспечивает качество сам, но создаёт процессы для его достижения.',kp:['Quality Planning — определение стандартов и как их достичь','Quality Assurance (QA) — аудит процессов (правильно ли мы работаем?)','Quality Control (QC) — проверка результатов (правильный ли мы продукт делаем?)','Cost of Quality: Prevention + Appraisal + Failure costs'],sections:[{t:'Gold Plating',p:'Добавление функций сверх согласованных требований — даже из благих побуждений — нарушение scope. PM должен пресекать gold plating: клиент получает то, что заказал.'},{t:'Инструменты качества',p:'7 инструментов качества: Cause-and-Effect Diagram (Ishikawa/Fishbone), Control Chart, Checklist, Pareto Chart, Histogram, Scatter Diagram, Flowchart.'}],info:{type:'note',text:'Стоимость исправления дефекта в конце проекта в 10-100 раз выше, чем на этапе планирования. Качество дешевле строить с нуля, чем исправлять.'},terms:[{w:'QA vs QC',d:'QA — проверка правильности процессов (превентивно), QC — проверка результатов (реактивно)'},{w:'Defect',d:'Отклонение результата от требований, требующее исправления'},{w:'Fitness for Use',d:'Продукт подходит для использования по назначению'},{w:'Gold Plating',d:'Добавление незапрошенных функций или улучшений сверх согласованного scope'}]},
  a18:{title:'Закрытие проекта и ретроспектива',tag:'Процессы',badge:'process',intro:'Закрытие — последняя группа процессов, которую часто недооценивают. Правильное закрытие проекта обеспечивает передачу результатов, архивирование знаний и официальное завершение.',kp:['Formal acceptance — официальное принятие результатов от заказчика','Lessons Learned — документ с опытом для будущих проектов','Administrative closure — архив документов, закрытие контрактов, освобождение ресурсов','Ретроспектива — обязательный элемент Agile-закрытия каждого Sprint'],sections:[{t:'Lessons Learned',p:'Один из самых ценных, но редко применяемых инструментов. Фиксирует что работало, что нет, что сделать иначе. Должны создаваться в течение всего проекта, а не только в конце.'},{t:'Agile Retrospective',p:'Sprint Retro — одно из 5 Scrum-событий. Команда отвечает: что хорошо, что плохо, что изменим в следующем Sprint. Формат Start / Stop / Continue — один из самых популярных.'}],info:{type:'tip',text:'Junior PM, который умеет проводить хорошую ретроспективу и ведёт реестр lessons learned — редкость. Это сразу выделяет тебя среди других кандидатов.'},terms:[{w:'Lessons Learned',d:'Знания, полученные в ходе проекта — задокументированный опыт для будущих проектов'},{w:'Formal Acceptance',d:'Официальное подтверждение заказчика, что результаты проекта соответствуют требованиям'},{w:'Retrospective',d:'Событие в конце Sprint для анализа процессов и улучшений в следующем Sprint'},{w:'Transition Plan',d:'План передачи результатов проекта в операционное использование'}]}
};

const navColors = {theory:'#185FA5',practice:'#BA7517',tools:'#534AB7',process:'#D85A30'};

let done = new Set();
function loadDone() { try { const d = localStorage.getItem(STORAGE_KEY); if(d) done = new Set(JSON.parse(d)); } catch(e){} }
function saveDone() { try { localStorage.setItem(STORAGE_KEY, JSON.stringify([...done])); } catch(e){} }

function updateStats() {
  const n = done.size, total = topics.length, pct = Math.round(n/total*100);
  document.getElementById('s-done').textContent = n;
  document.getElementById('s-pct').textContent = pct + '%';
  document.getElementById('s-left').textContent = total - n;
  document.getElementById('s-bar').style.width = pct + '%';
}

function renderTracker() {
  const body = document.getElementById('tracker-body');
  body.innerHTML = '';
  [1,2,3,4].forEach(g => {
    const label = document.createElement('div');
    label.className = 'section-label';
    label.textContent = groups[g];
    body.appendChild(label);
    topics.filter(t => t.g === g).forEach(t => {
      const isDone = done.has(t.id);
      const card = document.createElement('div');
      card.className = 'topic-card' + (isDone ? ' done' : '');
      card.innerHTML = `
        <div class="check">
          <svg class="check-icon" width="13" height="13" viewBox="0 0 13 13" fill="none">
            <path d="M2.5 6.5L5.5 9.5L10.5 3.5" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <div class="topic-body">
          <div class="topic-name">${t.name}</div>
          <div class="topic-meta">${t.meta}</div>
        </div>
        <span class="badge b-${t.badge}">${badgeLabel[t.badge]}</span>
        <button class="read-btn" onclick="event.stopPropagation();openArt('${t.art}')">Читать →</button>
      `;
      card.addEventListener('click', () => {
        if(done.has(t.id)) done.delete(t.id); else done.add(t.id);
        saveDone(); updateStats(); renderTracker(); syncDoneBtns();
      });
      body.appendChild(card);
    });
  });
}

function buildLibrary() {
  const nav = document.getElementById('lib-nav');
  const body = document.getElementById('lib-body');
  nav.innerHTML = ''; body.innerHTML = '';
  topics.forEach((t, i) => {
    const btn = document.createElement('button');
    btn.className = 'lib-nav-item' + (i===0?' active':'');
    btn.setAttribute('data-art', t.art);
    const isMobile = window.innerWidth < 700;
    if(isMobile) {
      btn.textContent = t.name.split('—')[0].split('(')[0].trim().substring(0,20);
    } else {
      btn.innerHTML = `<span class="nav-dot" style="background:${navColors[t.badge]}"></span>${t.name}`;
    }
    btn.onclick = () => { document.querySelectorAll('.lib-nav-item').forEach(n=>n.classList.remove('active')); btn.classList.add('active'); showArt(t.art); };
    nav.appendChild(btn);

    const art = articles[t.art];
    if(!art) return;
    const div = document.createElement('div');
    div.className = 'lib-article' + (i===0?' active':'');
    div.id = 'art-' + t.art;

    const phasesHTML = art.phases ? `<div class="phases">${art.phases.map((p,pi)=>`<div class="phase"><div class="phase-n">${pi+1}</div><div class="phase-name">${p}</div></div>`).join('')}</div>` : '';
    const sectionsHTML = (art.sections||[]).map(s=>`<div class="sub-sec"><div class="sub-title">${s.t}</div><div class="sub-text">${s.p}</div></div>`).join('');
    const infoHTML = art.info ? `<div class="info-box ib-${art.info.type}">${art.info.text}</div>` : '';
    const termsHTML = art.terms ? `<div class="terms-label">Глоссарий раздела</div><div class="terms-grid">${art.terms.map(tr=>`<div class="term"><div class="term-word">${tr.w}</div><div class="term-def">${tr.d}</div></div>`).join('')}</div>` : '';
    const isDone = done.has(t.id);

    div.innerHTML = `
      <div class="art-title">${art.title}</div>
      <span class="art-tag badge b-${art.badge}">${art.tag}</span>
      <div class="art-intro">${art.intro}</div>
      <div class="key-points">
        <div class="kp-label">Ключевые тезисы</div>
        ${(art.kp||[]).map(k=>`<div class="kp-row"><span class="kp-dot"></span><span>${k}</span></div>`).join('')}
      </div>
      ${phasesHTML}
      ${sectionsHTML}
      ${infoHTML}
      ${termsHTML}
      <button class="mark-btn ${isDone?'done-state':''}" id="mbtn-${t.id}" onclick="markDone('${t.id}',this)">
        ${isDone ? '✓ Тема изучена' : 'Отметить как изученное'}
      </button>
    `;
    body.appendChild(div);
  });
}

function markDone(id, btn) {
  if(done.has(id)) return;
  done.add(id); saveDone(); updateStats(); renderTracker();
  btn.className = 'mark-btn done-state'; btn.textContent = '✓ Тема изучена';
}

function syncDoneBtns() {
  topics.forEach(t => {
    const btn = document.getElementById('mbtn-'+t.id);
    if(!btn) return;
    const isDone = done.has(t.id);
    btn.className = 'mark-btn' + (isDone?' done-state':'');
    btn.textContent = isDone ? '✓ Тема изучена' : 'Отметить как изученное';
  });
}

function showArt(artId) {
  document.querySelectorAll('.lib-article').forEach(a=>a.classList.remove('active'));
  const el = document.getElementById('art-'+artId);
  if(el) { el.classList.add('active'); el.scrollIntoView({behavior:'smooth',block:'start'}); }
}

function openArt(artId) {
  switchTab('library');
  const navItem = document.querySelector(`[data-art="${artId}"]`);
  if(navItem) { document.querySelectorAll('.lib-nav-item').forEach(n=>n.classList.remove('active')); navItem.classList.add('active'); }
  showArt(artId);
}

function switchTab(tab) {
  document.querySelectorAll('.tab').forEach((t,i)=>t.classList.toggle('active',(i===0&&tab==='tracker')||(i===1&&tab==='library')));
  document.getElementById('tab-tracker').classList.toggle('active',tab==='tracker');
  document.getElementById('tab-library').classList.toggle('active',tab==='library');
}

loadDone();
renderTracker();
updateStats();
buildLibrary();
</script>
</body>
</html>
