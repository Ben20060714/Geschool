<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { gsap } from "gsap";
import {
  LayoutDashboard,
  GraduationCap,
  CalendarCheck,
  WalletCards,
  ChartNoAxesCombined,
  Search,
  Bell,
  ChevronDown,
  ArrowUpRight,
  Users,
  UserRoundCheck,
  BookOpenCheck,
  CircleDollarSign,
  MoreHorizontal,
  Plus,
  SlidersHorizontal,
  Download,
  Check,
  Clock3,
  X,
  Menu,
  Sparkles,
  TrendingUp,
  CircleHelp,
} from "lucide-vue-next";

type Section = "Dashboard" | "Grades" | "Attendance" | "Tuition" | "Statistics";
const sectionLabels: Record<Section, string> = {
  Dashboard: "Tableau de bord",
  Grades: "Notes",
  Attendance: "Présences",
  Tuition: "Frais de scolarité",
  Statistics: "Statistiques",
};
const active = ref<Section>("Dashboard");
const mobileOpen = ref(false);
const search = ref("");
const attendance = ref<Record<string, string>>({
  Ava: "Present",
  Noah: "Late",
  Mia: "Present",
  Laura: "Absent",
  Zoe: "Present",
});
const sections: { name: Section; icon: any }[] = [
  { name: "Dashboard", icon: LayoutDashboard },
  { name: "Grades", icon: GraduationCap },
  { name: "Attendance", icon: CalendarCheck },
  { name: "Tuition", icon: WalletCards },
  { name: "Statistics", icon: ChartNoAxesCombined },
];
const students = [
  {
    name: "Ava Kalenga",
    initials: "AT",
    class: "4e · A",
    assignment: "Mathématiques",
    score: 92,
    trend: "+4.2%",
    color: "coral",
  },
  {
    name: "Noah Muteba",
    initials: "NW",
    class: "5e · B",
    assignment: "Sciences",
    score: 87,
    trend: "+2.8%",
    color: "blue",
  },
  {
    name: "Mia Sakina",
    initials: "MA",
    class: "3e · A",
    assignment: "Littérature anglaise",
    score: 78,
    trend: "-1.4%",
    color: "gold",
  },
  {
    name: "Laura Martinez",
    initials: "LM",
    class: "4e · C",
    assignment: "Histoire",
    score: 96,
    trend: "+6.1%",
    color: "green",
  },
];
const attendanceRows = computed(() =>
  students.map((s) => ({
    ...s,
    status: attendance.value[s.name.split(" ")[0]] || "Present",
  }))
);
const filteredStudents = computed(() =>
  students.filter((s) =>
    `${s.name} ${s.assignment}`.toLowerCase().includes(search.value.toLowerCase())
  )
);
const setStatus = (first: string, status: string) => {
  attendance.value = { ...attendance.value, [first]: status };
};
const statusLabels: Record<string, string> = {
  Present: "Présent",
  Late: "En retard",
  Absent: "Absent",
};
const nav = (name: Section) => {
  active.value = name;
  mobileOpen.value = false;
};
onMounted(() => {
  gsap.from(".app-shell", { opacity: 0, y: 12, duration: 0.55, ease: "power2.out" });
});
</script>

<template>
  <div class="app-shell min-h-screen bg-[#f5f7fb] text-[#17213b]">
    <aside :class="['sidebar', { 'sidebar-open': mobileOpen }]">
      <div class="brand">
        <div class="brand-mark">
          <span></span>
          <span></span>
          <span></span>
        </div>
        <span>Ge<span>School</span></span>
      </div>
      <div class="school-switch">
        <div class="school-logo">W</div>
        <div>
          <strong>Masomo Academy</strong>
          <small>Espace Admin.</small>
        </div>
        <ChevronDown :size="15" />
      </div>
      <nav class="nav-list" aria-label="Navigation principale">
        <p class="nav-label">Espace de travail</p>
        <button v-for="item in sections" :key="item.name" :class="['nav-item', { active: active === item.name }]" @click="nav(item.name)">
          <component :is="item.icon" :size="19" />
            <span>{{ sectionLabels[item.name] }}</span>
            <span v-if="item.name === 'Attendance'" class="nav-dot"></span>
        </button>
      </nav>
      <div class="sidebar-bottom">
        <div class="upgrade">
          <Sparkles :size="18" /><strong>Aller plus loin</strong>
          <p>Obtenez des analyses plus détaillées avec Pro.</p>
          <button>Découvrir les offres <ArrowUpRight :size="14" /></button>
        </div>
        <button class="help"><CircleHelp :size="18" /> Centre d’aide</button>
        <div class="user-mini">
          <div class="avatar purple">JD</div>
          <div><strong>Jordan Davis</strong><small>Administrateur scolaire</small></div>
          <MoreHorizontal :size="18" />
        </div>
      </div>
    </aside>
    <div v-if="mobileOpen" class="scrim" @click="mobileOpen = false"></div>
    <main class="main-content">
      <header class="topbar">
        <button class="mobile-menu" @click="mobileOpen = true">
          <Menu :size="21" />
        </button>
        <div class="crumb">
          <span>Espace de travail</span><span>/</span><strong>{{ sectionLabels[active] }}</strong>
        </div>
        <div class="top-actions">
          <div class="search-box">
            <Search :size="17"/><input v-model="search" placeholder="Rechercher..." aria-label="Rechercher"/><kbd>⌘ K</kbd>
          </div>
          <button class="icon-button notification"><Bell :size="19" /><i></i></button>
          <div class="top-avatar">JD</div>
        </div>
      </header>
      <Transition name="fade" mode="out-in"
        ><div :key="active" class="page-wrap">
          <section class="page-heading">
            <div>
              <p class="eyebrow">
                {{
                  active === "Dashboard"
                    ? "Mardi 24 octobre 2024"
                    : "Vue d’ensemble de l’espace de travail"
                }}
              </p>
              <h1>{{ active === "Dashboard" ? "Bonjour Jordan" : active === "Grades" ? "Notes" : active === "Attendance" ? "Présences" : active === "Tuition" ? "Frais de scolarité" : "Statistiques" }}</h1>
              <p class="subheading">
                {{
                  active === "Dashboard"
                    ? "Voici ce qui se passe dans votre établissement aujourd’hui."
                    : `Suivez et gérez les ${active === "Grades" ? "notes" : active === "Attendance" ? "présences" : active === "Tuition" ? "frais de scolarité" : "statistiques"} de votre établissement au même endroit.`
                }}
              </p>
            </div>
            <div class="heading-actions">
              <button class="button ghost"><Download :size="16" /> Exporter</button>
              <button class="button primary">
                <Plus :size="17" />
                {{ active === "Attendance" ? "Faire l’appel" : "Ajouter" }}
              </button>
            </div>
          </section>
          <div v-if="active === 'Dashboard'" class="dashboard-view">
            <section class="kpi-grid">
              <article class="kpi-card">
                <div class="kpi-icon indigo"><Users :size="19" /></div>
                <div class="kpi-title">Total des élèves <ArrowUpRight :size="15" /></div>
                <strong>1,248</strong>
                <p><b class="up">+8.2%</b> <span>Ce mois-ci</span></p>
                <div class="sparkline indigo-line">
                  <span
                    v-for="h in [20, 27, 22, 34, 28, 42, 37, 51, 44, 61, 56, 75]"
                    :key="h"
                    :style="{ height: h + '%' }"
                  ></span>
                </div>
              </article>
              <article class="kpi-card">
                <div class="kpi-icon emerald"><UserRoundCheck :size="19" /></div>
                <div class="kpi-title">Taux de présence <ArrowUpRight :size="15" /></div>
                <strong>94.8%</strong>
                <p><b class="up">+1.6%</b> <span>Ce mois-ci</span></p>
                <div class="sparkline green-line">
                  <span
                    v-for="h in [34, 45, 38, 49, 42, 57, 48, 63, 57, 69, 64, 78]"
                    :key="h"
                    :style="{ height: h + '%' }"
                  ></span>
                </div>
              </article>
              <article class="kpi-card tuition-kpi">
                <div class="kpi-icon amber"><CircleDollarSign :size="19" /></div>
                <div class="kpi-title">Frais collectés <ArrowUpRight :size="15" /></div>
                <strong>$428,650</strong>
                <p><b class="up">+12.4%</b> <span>Ce mois-ci</span></p>
                <div class="ring-wrap">
                  <svg viewBox="0 0 42 42">
                    <circle class="ring-bg" cx="21" cy="21" r="15.5" />
                    <circle class="ring-fill" cx="21" cy="21" r="15.5" /></svg
                  ><b>78%</b>
                </div>
              </article>
              <article class="kpi-card">
                <div class="kpi-icon coral"><BookOpenCheck :size="19" /></div>
                <div class="kpi-title">Classes actives <ArrowUpRight :size="15" /></div>
                <strong>48</strong>
                <p><b class="neutral">+2</b> <span>Ce trimestre</span></p>
                <div class="mini-bars">
                  <i
                    v-for="h in [38, 52, 43, 68, 57, 83, 70, 92]"
                    :key="h"
                    :style="{ height: h + '%' }"
                  ></i>
                </div>
              </article>
            </section>
            <section class="content-grid">
              <article class="panel grades-panel">
                <div class="panel-head">
                  <div>
                    <h2>Notes des élèves</h2>
                    <p>Derniers résultats scolaires de toutes les classes</p>
                  </div>
                  <button class="text-button" @click="active = 'Grades'">
                    Tout voir <ArrowUpRight :size="15" />
                  </button>
                </div>
                <div class="table-wrap">
                  <table>
                    <thead>
                      <tr>
                        <th>Élève</th>
                        <th>Matière</th>
                        <th>Résultat</th>
                        <th>Tendance</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="s in students.slice(0, 3)" :key="s.name">
                        <td>
                          <div class="person">
                            <div :class="['avatar', s.color]">{{ s.initials }}</div>
                            <div>
                              <strong>{{ s.name }}</strong
                              ><small>{{ s.class }}</small>
                            </div>
                          </div>
                        </td>
                        <td>{{ s.assignment }}</td>
                        <td>
                          <div class="score">
                            <b>{{ s.score }}%</b>
                            <div class="progress">
                              <i :style="{ width: s.score + '%' }"></i>
                            </div>
                          </div>
                        </td>
                        <td>
                          <span :class="s.trend.startsWith('+') ? 'up' : 'down'">{{
                            s.trend
                          }}</span>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </article>
              <article class="panel attendance-panel">
                <div class="panel-head">
                  <div>
                    <h2>Présences du jour</h2>
                    <p>Lundi 24 octobre</p>
                  </div>
                  <button class="round-button"><MoreHorizontal :size="18" /></button>
                </div>
                <div class="attendance-overview">
                  <div class="attendance-circle">
                    <svg viewBox="0 0 100 100">
                      <circle cx="50" cy="50" r="41" />
                      <circle class="att-fill" cx="50" cy="50" r="41" /></svg
                    ><strong>94.8<small>%</small></strong
                    ><span>Présents aujourd’hui</span>
                  </div>
                  <div class="legend">
                    <div>
                      <i class="dot green-dot"></i><span>Présents</span><b>1 184</b>
                    </div>
                    <div><i class="dot amber-dot"></i><span>En retard</span><b>38</b></div>
                    <div><i class="dot red-dot"></i><span>Absents</span><b>26</b></div>
                  </div>
                </div>
                <button class="full-button" @click="active = 'Attendance'">
                  Gérer les présences <ArrowUpRight :size="15" />
                </button>
              </article>
            </section>
            <section class="bottom-grid">
              <article class="panel chart-panel">
                <div class="panel-head">
                  <div>
                    <h2>Vue d’ensemble des présences</h2>
                    <p>Taux de présence quotidien sur les 7 derniers jours</p>
                  </div>
                  <button class="select-button">
                    Cette semaine <ChevronDown :size="15" />
                  </button>
                </div>
                <div class="chart">
                  <div class="y-labels">
                    <span>100%</span><span>95%</span><span>90%</span><span>85%</span>
                  </div>
                  <svg viewBox="0 0 700 220" preserveAspectRatio="none">
                    <defs>
                      <linearGradient id="chartFill" x1="0" x2="0" y1="0" y2="1">
                        <stop offset="0" stop-color="#5664e8" stop-opacity=".2" />
                        <stop offset="1" stop-color="#5664e8" stop-opacity="0" />
                      </linearGradient>
                    </defs>
                    <path
                      class="area"
                      d="M0,104 C45,110 58,77 105,87 S155,125 210,99 S260,76 315,89 S365,54 420,74 S475,102 525,69 S580,35 630,58 S675,42 700,31 L700,220 L0,220 Z"
                    />
                    <path
                      class="line"
                      d="M0,104 C45,110 58,77 105,87 S155,125 210,99 S260,76 315,89 S365,54 420,74 S475,102 525,69 S580,35 630,58 S675,42 700,31"
                    />
                    <circle cx="630" cy="58" r="5" />
                  </svg>
                  <div class="x-labels">
                    <span>Lun</span><span>Mar</span><span>Mer</span><span>Jeu</span
                    ><span>Ven</span><span>Sam</span><span>Dim</span>
                  </div>
                </div>
              </article>
              <article class="panel events-panel">
                <div class="panel-head">
                  <div>
                    <h2>Événements à venir</h2>
                    <p>Gardez un œil sur les prochaines échéances</p>
                  </div>
                  <button class="round-button"><MoreHorizontal :size="18" /></button>
                </div>
                <div class="event">
                  <div class="date-block"><b>26</b><span>OCT</span></div>
                  <div>
                    <strong>Réunion parents-professeurs</strong
                    ><small>Jeudi · 9 h 00 – 16 h 00</small>
                  </div>
                  <ArrowUpRight :size="15" />
                </div>
                <div class="event">
                  <div class="date-block blue-date"><b>30</b><span>OCT</span></div>
                  <div>
                    <strong>Début des examens de mi-semestre</strong
                    ><small>Lundi · Toute la journée</small>
                  </div>
                  <ArrowUpRight :size="15" />
                </div>
                <button class="full-button secondary-button">
                  Voir le calendrier <ArrowUpRight :size="15" />
                </button>
              </article>
            </section>
          </div>
          <section v-else-if="active === 'Grades'" class="panel page-panel">
            <div class="panel-head">
              <div>
                <h2>Résultats scolaires</h2>
                <p>Suivez les résultats et les progrès de chaque élève</p>
              </div>
              <button class="select-button">
                <SlidersHorizontal :size="15" /> Filtrer
              </button>
            </div>
            <div class="table-wrap large-table">
              <table>
                <thead>
                  <tr>
                    <th>Élève</th>
                    <th>Matière</th>
                    <th>Résultat</th>
                    <th>Progression</th>
                    <th>Statut</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="s in filteredStudents" :key="s.name">
                    <td>
                      <div class="person">
                        <div :class="['avatar', s.color]">{{ s.initials }}</div>
                        <div>
                          <strong>{{ s.name }}</strong
                          ><small>{{ s.class }}</small>
                        </div>
                      </div>
                    </td>
                    <td>{{ s.assignment }}</td>
                    <td>
                      <b>{{ s.score }}%</b>
                    </td>
                    <td>
                      <div class="score">
                        <div class="progress wide">
                          <i :style="{ width: s.score + '%' }"></i>
                        </div>
                      </div>
                    </td>
                    <td>
                      <span
                        :class="[
                          'status-badge',
                          s.score > 90 ? 'success' : s.score > 80 ? 'info' : 'warning',
                        ]"
                        >{{
                          s.score > 90
                            ? "Excellent"
                            : s.score > 80
                            ? "En bonne voie"
                            : "À surveiller"
                        }}</span
                      >
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
          <section v-else-if="active === 'Attendance'" class="panel page-panel">
            <div class="panel-head">
              <div>
                <h2>Présences quotidiennes</h2>
                <p>Classe de 4e · Section A · Lundi 24 octobre</p>
              </div>
              <button class="select-button">Aujourd’hui <ChevronDown :size="15" /></button>
            </div>
            <div class="attendance-summary">
              <div>
                <span class="dot green-dot"></span><b>Présents</b><strong>24</strong>
              </div>
              <div><span class="dot amber-dot"></span><b>En retard</b><strong>2</strong></div>
              <div><span class="dot red-dot"></span><b>Absents</b><strong>1</strong></div>
            </div>
            <div class="attendance-list">
              <div v-for="row in attendanceRows" :key="row.name" class="attendance-row">
                <div class="person">
                  <div :class="['avatar', row.color]">{{ row.initials }}</div>
                  <div>
                    <strong>{{ row.name }}</strong
                    ><small>{{ row.class }}</small>
                  </div>
                </div>
                <div class="toggle-set">
                  <button
                    v-for="status in ['Present', 'Late', 'Absent']"
                    :key="status"
                    :class="[{ selected: row.status === status }, status.toLowerCase()]"
                    @click="setStatus(row.name.split(' ')[0], status)"
                  >
                    {{ status === "Present" ? "P" : status === "Late" ? "L" : "A" }}
                    <span>{{ statusLabels[status] }}</span>
                  </button>
                </div>
              </div>
            </div>
          </section>
          <section v-else-if="active === 'Tuition'" class="tuition-view">
            <div class="kpi-grid tuition-grid">
              <article class="kpi-card">
                <div class="kpi-icon indigo"><CircleDollarSign :size="19" /></div>
                <div class="kpi-title">Total facturé</div>
                <strong>$548,000</strong>
                <p><span>Trimestre d’automne 2024</span></p>
              </article>
              <article class="kpi-card">
                <div class="kpi-icon emerald"><Check :size="19" /></div>
                <div class="kpi-title">Collecté</div>
                <strong>$428,650</strong>
                <p><b class="up">78.2%</b> <span>du total facturé</span></p>
              </article>
              <article class="kpi-card">
                <div class="kpi-icon amber"><Clock3 :size="19" /></div>
                <div class="kpi-title">Impayé</div>
                <strong>$119,350</strong>
                <p><b class="warning-text">142 familles</b> <span>avec un solde dû</span></p>
              </article>
            </div>
            <article class="panel chart-panel">
              <div class="panel-head">
                <div>
                  <h2>Suivi des encaissements</h2>
                  <p>Frais de scolarité collectés et impayés par mois</p>
                </div>
                <button class="select-button">
                  Cette année <ChevronDown :size="15" />
                </button>
              </div>
              <div class="finance-chart">
                <div
                  v-for="(month, i) in [
                    'Jan',
                    'Fév',
                    'Mars',
                    'Avr',
                    'Mai',
                    'Juin',
                    'Juil',
                    'Août',
                    'Sept',
                    'Oct',
                  ]"
                  :key="month"
                  class="finance-col"
                >
                  <div class="bars">
                    <i
                      :style="{
                        height: [45, 58, 51, 68, 62, 74, 80, 67, 88, 94][i] + '%',
                      }"
                    ></i
                    ><i
                      :style="{
                        height: [22, 18, 29, 20, 27, 16, 12, 19, 10, 8][i] + '%',
                      }"
                    ></i>
                  </div>
                  <span>{{ month }}</span>
                </div>
              </div>
              <div class="chart-key">
                <span><i class="key-indigo"></i>Collecté</span
                ><span><i class="key-amber"></i>Impayé</span>
              </div>
            </article>
          </section>
          <section v-else class="stats-view">
            <div class="stats-cards">
              <article class="stat-highlight">
                <div>
                  <span>Indice de progression scolaire</span><strong>+18.6%</strong>
                  <p><TrendingUp :size="15" /> En hausse par rapport au trimestre dernier</p>
                </div>
                <div class="predictive-line">
                  <svg viewBox="0 0 260 90" preserveAspectRatio="none">
                    <path
                      d="M0 76 C25 70, 35 57, 58 63 S90 55, 110 58 S130 39, 154 45 S178 38, 192 41 S220 18, 260 8"
                    />
                  </svg>
                </div>
              </article>
              <article class="stat-highlight teal">
                <div>
                  <span>Engagement des élèves</span><strong>86.4%</strong>
                  <p><TrendingUp :size="15" /> +5.2% depuis septembre</p>
                </div>
                <div class="predictive-line">
                  <svg viewBox="0 0 260 90" preserveAspectRatio="none">
                    <path
                      d="M0 65 C30 74, 40 43, 66 53 S98 28, 120 42 S153 28, 178 34 S214 15, 260 20"
                    />
                  </svg>
                </div>
              </article>
            </div>
            <div class="stats-grid">
              <article class="panel stat-panel">
                <div class="panel-head">
                  <div>
                    <h2>Résultats par matière</h2>
                    <p>Répartition des moyennes par classe</p>
                  </div>
                </div>
                <div class="horizontal-bars">
                  <div
                    v-for="item in [
                      ['Mathématiques', 86, '#5967e9'],
                      ['Sciences', 82, '#3fb88d'],
                      ['Anglais', 78, '#eead54'],
                      ['Histoire', 74, '#f07868'],
                      ['Arts plastiques', 91, '#9c7eea'],
                    ]"
                    :key="item[0]"
                  >
                    <span>{{ item[0] }}</span>
                    <div>
                      <i :style="{ width: item[1] + '%', background: item[2] }"></i>
                    </div>
                    <b>{{ item[1] }}%</b>
                  </div>
                </div>
              </article>
              <article class="panel stat-panel donut-panel">
                <div class="panel-head">
                  <div>
                    <h2>Répartition des inscriptions</h2>
                    <p>Élèves par niveau</p>
                  </div>
                </div>
                <div class="donut-chart">
                  <svg viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="34" />
                    <circle class="d1" cx="50" cy="50" r="34" />
                    <circle class="d2" cx="50" cy="50" r="34" />
                    <circle class="d3" cx="50" cy="50" r="34" /></svg
                  ><strong>1 248<small>élèves</small></strong>
                </div>
                <div class="donut-legend">
                  <span><i class="dot dot-indigo"></i>Grade 6–7 <b>32%</b></span
                  ><span><i class="dot green-dot"></i>4e–3e <b>41%</b></span
                  ><span><i class="dot amber-dot"></i>2de–Terminale <b>27%</b></span>
                </div>
              </article>
            </div>
          </section>
        </div></Transition
      >
    </main>
  </div>
</template>
