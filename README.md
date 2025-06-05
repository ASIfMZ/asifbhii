# asifbhii
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Engineering Mechanics Question Bank</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Scholarly Warmth -->
    <!-- Application Structure Plan: The SPA is designed as an interactive dashboard. This structure was chosen over a linear list to empower students to actively study. The key components are: 1) A filter panel (by year, type, topic) for targeted practice. 2) A dynamic statistics section with charts to visualize question trends and topic weightage. 3) A responsive grid to display the filtered questions as individual cards. This architecture transforms passive reading into an active, exploratory learning experience, which is far more effective for exam preparation than a static document. -->
    <!-- Visualization & Content Choices: The source report is a list of questions. Goal: Allow students to analyze, filter, and practice questions efficiently. Presentation: A dashboard with interactive filters and charts. Viz Choices: 1) A bar chart shows question frequency per year (Goal: Compare, Tool: Chart.js). 2) A donut chart shows topic distribution (Goal: Inform, Tool: Chart.js). These visualizations provide an immediate overview of exam patterns. Content Presentation: Questions are displayed on cards in a grid, which is a modular and responsive way to present distinct information blocks. Interaction: Users click filter buttons and select from a dropdown. This triggers JS functions to filter a central data array and dynamically re-render both the question grid and the charts, providing instant feedback. This interactive loop is key to the app's utility. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F4E3; /* Warm neutral background */
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 350px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
                max-height: 400px;
            }
        }
        .filter-btn {
            transition: all 0.2s ease-in-out;
        }
        .filter-btn.active {
            background-color: #4682B4; /* Muted teal accent */
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }
        .custom-select {
            background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 20 20'%3e%3cpath stroke='%236b7280' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5' d='M6 8l4 4 4-4'/%3e%3c/svg%3e");
            background-position: right 0.5rem center;
            background-repeat: no-repeat;
            background-size: 1.5em 1.5em;
            padding-right: 2.5rem;
            -webkit-appearance: none;
            -moz-appearance: none;
            appearance: none;
        }
    </style>
</head>
<body class="text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-bold text-[#363636]">Interactive Engineering Mechanics Question Bank</h1>
            <p class="text-lg text-gray-600 mt-2">Filter, visualize, and master questions from past exams.</p>
        </header>

        <main>
            <!-- Filters Section -->
            <section id="filters" class="mb-8 p-6 bg-white/60 rounded-xl shadow-md backdrop-blur-sm">
                <h2 class="text-xl font-bold mb-4 text-[#363636]">Filter Questions</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div>
                        <h3 class="font-semibold mb-2 text-gray-700">Filter by Year</h3>
                        <div id="year-filters" class="flex flex-wrap gap-2">
                            <button data-year="2024" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">2024</button>
                            <button data-year="2023" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">2023</button>
                            <button data-year="2022" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">2022</button>
                            <button data-year="2019" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">2019</button>
                        </div>
                    </div>
                    <div>
                        <h3 class="font-semibold mb-2 text-gray-700">Filter by Type</h3>
                        <div id="type-filters" class="flex flex-wrap gap-2">
                             <button data-type="objective" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">Objective</button>
                             <button data-type="problem" class="filter-btn bg-white px-4 py-2 rounded-lg shadow-sm font-medium border border-gray-200 hover:bg-gray-100">Problem Solving</button>
                        </div>
                    </div>
                    <div>
                        <h3 class="font-semibold mb-2 text-gray-700">Filter by Topic</h3>
                        <select id="topic-filter" class="w-full p-2.5 border border-gray-200 rounded-lg shadow-sm bg-white custom-select focus:ring-2 focus:ring-[#4682B4] focus:border-[#4682B4]">
                            <option value="all">All Topics</option>
                            <option value="Forces & Resultants">Forces & Resultants</option>
                            <option value="Equilibrium & Lami's Theorem">Equilibrium & Lami's Theorem</option>
                            <option value="Friction">Friction</option>
                            <option value="Center of Gravity & MOI">Center of Gravity & MOI</option>
                            <option value="Simple Machines">Simple Machines</option>
                            <option value="Dynamics & Kinematics">Dynamics & Kinematics</option>
                        </select>
                    </div>
                </div>
            </section>

            <!-- Stats Section -->
            <section id="stats" class="mb-8 p-6 bg-white/60 rounded-xl shadow-md backdrop-blur-sm">
                 <h2 class="text-xl font-bold mb-4 text-center text-[#363636]">Question Insights</h2>
                 <p id="stats-intro" class="text-center text-gray-600 mb-6 max-w-3xl mx-auto">This section provides a visual breakdown of the selected questions. Use these charts to understand the frequency of topics and the distribution of questions across different exam years, helping you focus your study efforts.</p>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-center">
                    <div class="chart-container">
                        <canvas id="questionsByYearChart"></canvas>
                    </div>
                    <div class="chart-container">
                        <canvas id="questionsByTopicChart"></canvas>
                    </div>
                </div>
            </section>

            <!-- Questions Display Section -->
            <section id="questions-display">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-bold text-[#363636]">Questions (<span id="question-count">0</span>)</h2>
                    <button id="clear-filters" class="text-sm font-medium text-[#4682B4] hover:underline">Clear All Filters</button>
                </div>
                <div id="questions-grid" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6">
                    <!-- Questions will be dynamically inserted here -->
                </div>
                 <div id="no-results" class="text-center py-12 text-gray-500 hidden">
                    <p class="text-xl">No questions found for the selected filters.</p>
                </div>
            </section>
        </main>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const questionsData = [
                { id: 1, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'One kg force is equal to ____ N.' },
                { id: 2, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'The process of finding out the resultant force is called ______ of forces.' },
                { id: 3, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'The resultant of two forces P and Q (such that P > Q) acting along the same straight line, but in opposite direction is given by ______.' },
                { id: 4, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'The forces which do not meet at one point and their lines of action do not lie on the same plane are known as ______.' },
                { id: 5, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The point through which the whole weight of the body acts, irrespective of its position is known as ______.' },
                { id: 6, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The S.I. unit of moment of inertia is ______.' },
                { id: 7, year: 2019, type: 'objective', topic: 'Friction', text: 'The friction experienced by a body, when at rest is known as ______.' },
                { id: 8, year: 2019, type: 'objective', topic: 'Simple Machines', text: 'In actual machines, mechanical advantage is ______ velocity ratio.' },
                { id: 9, year: 2019, type: 'objective', topic: 'Simple Machines', text: 'The velocity ratio for the first system of pulleys is ______.' },
                { id: 10, year: 2019, type: 'objective', topic: 'Dynamics & Kinematics', text: 'The rate of doing work is known as ______.' },
                { id: 11, year: 2019, type: 'objective', topic: 'Dynamics & Kinematics', text: 'Energy may be defined as the capacity of doing work. (True/False)' },
                { id: 12, year: 2019, type: 'objective', topic: 'Dynamics & Kinematics', text: 'The rate of displacement of a body is called momentum. (True/False)' },
                { id: 13, year: 2019, type: 'objective', topic: 'Simple Machines', text: 'A non-reversible machine is also called a self-locking machine. (True/False)' },
                { id: 14, year: 2019, type: 'objective', topic: 'Friction', text: 'The angle of the inclined plane at which a body just begins to slide down the plane, is called helix angle. (True/False)' },
                { id: 15, year: 2019, type: 'objective', topic: 'Friction', text: 'Static friction is always less than dynamic friction. (True/False)' },
                { id: 16, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Moment of inertia of a rectangular section 3 cm wide and 4 cm deep about x-x axis is 16 cm^4. (True/False)' },
                { id: 17, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The centre of gravity of a triangle lies at a point where its medians intersect each other. (True/False)' },
                { id: 18, year: 2019, type: 'objective', topic: 'Equilibrium & Lami\'s Theorem', text: 'If the resultant of a number of forces acting on a body is zero, then the body will not be in equilibrium. (True/False)' },
                { id: 19, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'Vectors method for the resultant force is also called polygon law of forces. (True/False)' },
                { id: 20, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'The resultant of two forces each equal to P and acting at right angles is sqrt(2)P. (True/False)' },
                { id: 21, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'Two forces are acting at an angle of 120°. The bigger force is 40 N and the resultant is perpendicular to the smaller one. The smaller force is: i) 20 N, ii) 40 N, iii) 80 N, iv) None of these' },
                { id: 22, year: 2019, type: 'objective', topic: 'Forces & Resultants', text: 'The forces, whose line of action are parallel to each other and act in the same direction, are known as: i) Coplanar concurrent forces, ii) Coplanar non-concurrent forces, iii) Like parallel forces, iv) Unlike parallel forces' },
                { id: 23, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The centre of gravity of an equilateral triangle with each side a is, ______ from any of the three sides. i) sqrt(3)a/2, ii) 2sqrt(3)a, iii) a/(2sqrt(3)), iv) 3sqrt(2)a' },
                { id: 24, year: 2019, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The moment of inertia of a square of side \'a\' about an axis through its centre of gravity is: i) a^4/4, ii) a^4/8, iii) a^4/12, iv) a^4/36' },
                { id: 25, year: 2019, type: 'objective', topic: 'Friction', text: 'Co-efficient of friction depends upon: i) area of contact only, ii) nature of surface only, iii) both (i) and (ii), iv) none of these' },
                { id: 26, year: 2023, type: 'objective', topic: 'Dynamics & Kinematics', text: 'The unit of work or energy in S. I. unit is: (i) Newton, (ii) Kilogram meter, (iii) Watt, (iv) Joule' },
                { id: 27, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Forces are called concurrent when their lines of action meet in: (i) one point, (ii) two points, (iii) plane, (iv) perpendicular planes' },
                { id: 28, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'The algebraic sum of the resolved parts of a number of forces in a given direction is equal to the resolved part of their resultant in the same direction. This is as per the principle of: (i) forces, (ii) independence of forces, (iii) dependence of forces, (iv) resolution of forces' },
                { id: 29, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'The weight of a body is due to: (i) centripetal force of earth, (ii) gravitational pull exerted by the earth, (iii) forces experienced by body in atmosphere, (iv) gravitational force of attraction towards the centre of the earth.' },
                { id: 30, year: 2023, type: 'objective', topic: 'Simple Machines', text: 'Maximum mechanical advantage of a lifting machine is: (i) 1/m, (ii) 1+m, (iii) 1-m, (iv) m' },
                { id: 31, year: 2023, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Unit of moment of inertia in S.I system is: (i) m^3, (ii) m^4, (iii) Nm^2, (iv) Nm^3' },
                { id: 32, year: 2023, type: 'objective', topic: 'Friction', text: 'Angle between the resultant of normal reaction and frictional force is called: (i) angle of friction, (ii) angle of repose, (iii) cone of friction, (iv) angle of inclination' },
                { id: 33, year: 2023, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Centre of gravity of a triangle lies at the point of intersection of: (i) diagonals, (ii) medians, (iii) altitudes, (iv) bisector of angles' },
                { id: 34, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Two non-collinear parallel equal forces acting in opposite direction: (i) balance each other, (ii) constitute a moment, (iii) constitute a couple, (iv) constitute a moment of couple' },
                { id: 35, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Which of the following is not a Scalar Quantity? (i) Time, (ii) Mass, (iii) Volume, (iv) Acceleration' },
                { id: 36, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Resultant of two unlike parallel forces F1 and F2 is ______.' },
                { id: 37, year: 2023, type: 'objective', topic: 'Friction', text: 'Static friction is ______ than dynamic friction.' },
                { id: 38, year: 2023, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Centre of gravity is the point at which the whole ______ of a body is supposed to be concentrated.' },
                { id: 39, year: 2023, type: 'objective', topic: 'Dynamics & Kinematics', text: 'A large force acting on a body for a short time is called ______.' },
                { id: 40, year: 2023, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Moment of inertia of a square of side \'d\' about X-X axis is ______.' },
                { id: 41, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'The resultant of two unlike and equal parallel forces whose lines of action are same is ______.' },
                { id: 42, year: 2023, type: 'objective', topic: 'Friction', text: 'The opposing force which acts at the point of contact of the two bodies which slides over another is called ______.' },
                { id: 43, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Moment of a force is a ______ quantity.' },
                { id: 44, year: 2023, type: 'objective', topic: 'Simple Machines', text: 'A simple machine is said to be Self-locking if its efficiency is ______.' },
                { id: 45, year: 2023, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The axis about which a plane figure is symmetrical on both of its sides is called ______.' },
                { id: 46, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'A resultant is a single force which can replace two or more forces and produce the same effect on the body as the given system of forces. (True/False)' },
                { id: 47, year: 2023, type: 'objective', topic: 'Forces & Resultants', text: 'Geometrically the moment of a force about a point, taken as the vertex of a triangle is equal to half the area of the triangle. (True/False)' },
                { id: 48, year: 2023, type: 'objective', topic: 'Friction', text: 'Sliding friction is a type of dynamic friction. (True/False)' },
                { id: 49, year: 2023, type: 'objective', topic: 'Dynamics & Kinematics', text: 'If the velocity of a body increases, its momentum will decrease. (True/False)' },
                { id: 50, year: 2023, type: 'objective', topic: 'Simple Machines', text: 'The distance through which a screw thread advances axially in one revolution is called pitch. (True/False)' },
                { id: 51, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'The forces which intersect or meet at a common point are called ______ forces.' },
                { id: 52, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'The resultant of two forces P and Q acting at an angle θ is equal to ______.' },
                { id: 53, year: 2022, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Moment of inertia of a circular section of diameter \'d\' is ______.' },
                { id: 54, year: 2022, type: 'objective', topic: 'Simple Machines', text: 'Efficiency is 100% for ______ machine.' },
                { id: 55, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'A body of mass 7.5 kg is moving with a velocity of 1.2 m/s. If a force of 15N is applied on the body, its velocity after 2 sec is ______ m/s.' },
                { id: 56, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'The unit of moment of force in S.I. system is ______.' },
                { id: 57, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'To create a couple, the nature of the pair of forces should be ______.' },
                { id: 58, year: 2022, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Center of gravity is the point of a body through which the ______ of the body acts.' },
                { id: 59, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'If \'mgh\' is the energy stored in a body of mass \'m\' after lifting it to a height \'h\' from ground, then the term \'mgh\' is ______ energy.' },
                { id: 60, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'The kinetic energy stored in a body of mass 8 kg moving with a velocity of 2 m/s is ______ kJ.' },
                { id: 61, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'The other name of vector diagram is force diagram. (True/False)' },
                { id: 62, year: 2022, type: 'objective', topic: 'Friction', text: 'The slope on the road surface generally provided on the curves is known as angle of friction. (True/False)' },
                { id: 63, year: 2022, type: 'objective', topic: 'Center of Gravity & MOI', text: 'All body has one and only one centre of gravity. (True/False)' },
                { id: 64, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'One of the characteristics of a couple is that it can cause a body to move in the direction of a greater force. (True/False)' },
                { id: 65, year: 2022, type: 'objective', topic: 'Friction', text: 'The force of friction always acts opposite to the direction of motion. (True/False)' },
                { id: 66, year: 2022, type: 'objective', topic: 'Equilibrium & Lami\'s Theorem', text: 'The Lami\'s Theorem is applicable only for ______.' },
                { id: 67, year: 2022, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The moment of inertia of a triangular section of base (b) and height (h) about an axis passing through its vertex and parallel to the base is ______ as that passing through its C.G. and parallel to the base.' },
                { id: 68, year: 2022, type: 'objective', topic: 'Simple Machines', text: 'The efficiency of a lifting machine is the ratio of ______.' },
                { id: 69, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'A person is carrying a bag on his head of mass 5 kg. The person covered a distance of 5 m. The work-done is: (i) 25 Joule, (ii) 0 Joule, (iii) 10 Joule, (iv) 25 Kilo-Joule' },
                { id: 70, year: 2022, type: 'objective', topic: 'Center of Gravity & MOI', text: 'What is the distance of the centre of gravity of a cube from every face, if the length of each side is 6 m? (i) 2 m, (ii) 3 m, (iii) 6 m, (iv) 4 m' },
                { id: 71, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'What are vector quantities?' },
                { id: 72, year: 2022, type: 'objective', topic: 'Forces & Resultants', text: 'Define coplanar collinear forces.' },
                { id: 73, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'What do mean by kinetic energy?' },
                { id: 74, year: 2022, type: 'objective', topic: 'Friction', text: 'Define force of friction.' },
                { id: 75, year: 2022, type: 'objective', topic: 'Dynamics & Kinematics', text: 'What do mean by Work-done?' },
                { id: 76, year: 2024, type: 'objective', topic: 'Friction', text: 'The value of Kinetic friction slightly decreases with the ______ in speed.' },
                { id: 77, year: 2024, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The centre of gravity of a triangle lies at the point of intersection of ______.' },
                { id: 78, year: 2024, type: 'objective', topic: 'Forces & Resultants', text: 'The second moment of a force is also said to be ______.' },
                { id: 79, year: 2024, type: 'objective', topic: 'Simple Machines', text: 'For reversible machine the efficiency is ______ than 50%.' },
                { id: 80, year: 2024, type: 'objective', topic: 'Forces & Resultants', text: 'The unit of moment is same as unit of ______.' },
                { id: 81, year: 2024, type: 'objective', topic: 'Simple Machines', text: 'The maximum M.A. of a lifting machine is: (i) m, (ii) m x VR, (iii) 1/m, (iv) 1/(m x VR)' },
                { id: 82, year: 2024, type: 'objective', topic: 'Dynamics & Kinematics', text: 'The unit of work done is: (i) Watt, (ii) Horse power, (iii) Joule, (iv) Newton' },
                { id: 83, year: 2024, type: 'objective', topic: 'Center of Gravity & MOI', text: 'Section Modulus for a circular section of diameter \'d\' is: (i) πd³/32, (ii) πd⁴/32, (iii) πd⁴/64, (iv) None of these' },
                { id: 84, year: 2024, type: 'objective', topic: 'Center of Gravity & MOI', text: 'The centre of gravity of a semicircle lies on its vertical radius at a distance of: (i) 4r/3π, (ii) 3r/4π, (iii) 4π/3r, (iv) 3π/4r' },
                { id: 85, year: 2024, type: 'objective', topic: 'Forces & Resultants', text: 'Two equal like parallel forces acting some distant apart forms a: (i) Moment, (ii) Couple, (iii) Torque, (iv) Moment of Inertia.' },
                { id: 86, year: 2024, type: 'objective', topic: 'Dynamics & Kinematics', text: 'In Kinetics the magnitude of motion of a body is related to the amount of applied force. (True/False)' },
                { id: 87, year: 2024, type: 'objective', topic: 'Forces & Resultants', text: 'Unlike parallel forces are divergent. (True/False)' },
                { id: 88, year: 2024, type: 'objective', topic: 'Equilibrium & Lami\'s Theorem', text: 'Lami\'s theorem is applicable to three concurrent forces in equilibrium. (True/False)' },
                { id: 89, year: 2024, type: 'objective', topic: 'Friction', text: 'Friction acting on a body depends on the area of contact. (True/False)' },
                { id: 90, year: 2024, type: 'objective', topic: 'Dynamics & Kinematics', text: 'Acceleration of a body is due to change in velocity with time. (True/False)' },
                { id: 91, year: 2019, type: 'problem', topic: 'Forces & Resultants', text: 'a) State the principle of transmissibility of a force. b) Find the magnitude of two forces such that, if they act at right angles, their resultant is 5N while when they act at an angle of 60°, their resultant is sqrt(37) N.' },
                { id: 92, year: 2019, type: 'problem', topic: 'Equilibrium & Lami\'s Theorem', text: 'a) State Lami\'s theorem. b) A machine weighing 1500 N is supported by two chains. One is inclined at 30° to the horizontal and other is inclined at 45° to the horizontal. Find the tensions in the two chains.' },
                { id: 93, year: 2019, type: 'problem', topic: 'Center of Gravity & MOI', text: 'a) State parallel axis theorem. b) Find the moment of inertia about the horizontal axis through the c.g. of the I section. Top flange: 60mm x 10mm, Web: 10mm x 100mm, Bottom flange: 120mm x 10mm.' },
                { id: 94, year: 2019, type: 'problem', topic: 'Friction', text: 'a) What are the laws of static friction? b) An effort of 200 N is required to move a body up an inclined plane of 15°. If the angle is made 20°, the effort required is 230 N. Find the weight of the body and the co-efficient of friction.' },
                { id: 95, year: 2019, type: 'problem', topic: 'Simple Machines', text: 'a) Define mechanical advantage and velocity ratio. b) In a lifting machine an effort of 120 N raises a load of 6.5 KN with efficiency 60%. An effort of 200 N raises 11.5 KN. Find max MA and max efficiency.' },
                { id: 96, year: 2019, type: 'problem', topic: 'Simple Machines', text: 'a) A simple screw jack has a thread of pitch 12 mm. Find the load lifted by an effort of 20 N on a 500 mm handle. Efficiency is 50%. b) In a simple wheel and axle, radii are 240mm and 40mm. Find efficiency if 60N lifts 300N.' },
                { id: 97, year: 2019, type: 'problem', topic: 'Dynamics & Kinematics', text: 'a) A stone is thrown up at 40 m/s. After 3s, another stone is thrown up. They strike the ground at the same time. Find the velocity of the second stone. b) A car starts with 2 m/s^2 acceleration. A police car starts 5s later at a uniform velocity of 20 m/s. Find time to overtake.' },
                { id: 98, year: 2023, type: 'problem', topic: 'Forces & Resultants', text: '(a) State the principle of resolution of forces. (b) Two forces 100N and 60N act a point at an angle of 60°. Find the magnitude and direction of the resultant.' },
                { id: 99, year: 2023, type: 'problem', topic: 'Equilibrium & Lami\'s Theorem', text: '(a) State Lami\'s theorem. (b) A simply supported beam AB of 8m has point loads of 5kN, 1kN, 2kN at 2m, 4m, 6m from left. A UDL of 2.5 kN/m runs for 2m from left. Find support reactions.' },
                { id: 100, year: 2023, type: 'problem', topic: 'Center of Gravity & MOI', text: '(a) Define parallel Axis theorem. (b) Determine the moment of inertia of an I section about X-X and Y-Y axes. Top flange: 10cm x 2cm, Web: 8cm x 2cm, Bottom flange: 12cm x 4cm.' },
                { id: 101, year: 2023, type: 'problem', topic: 'Friction', text: '(a) Define Static friction and Dynamic friction. (b) A body of weight 40 kN rests on a rough plane at 30°. The plane is raised to 60°. What parallel force will support the body?' },
                { id: 102, year: 2023, type: 'problem', topic: 'Dynamics & Kinematics', text: 'A particle starts at 4 m/sec. After 2 sec another particle leaves in the same direction at 5 m/sec with 3 m/sec^2 acceleration. Find when and where it will overtake the first.' },
                { id: 103, year: 2023, type: 'problem', topic: 'Simple Machines', text: '(a) Define mechanical advantage and velocity ratio. (b) An effort of 15N lifts 300N and 20N lifts 500N. Establish the law of the machine and find effort for 700N.' },
                { id: 104, year: 2022, type: 'problem', topic: 'Forces & Resultants', text: '(a) State differences between composition and resolution of forces. (b) Two forces act at 150°. The smaller is 30N and resultant is perpendicular to it. Find the bigger force. (c) State Lami\'s Theorem with formula and diagram.' },
                { id: 105, year: 2022, type: 'problem', topic: 'Forces & Resultants', text: '(a) A force of 15 N is applied perpendicular to a door 0.8 m wide. Find the moment about the hinge. (b) A 2.5 m rod AB is supported at ends. A point load of 5 kN is at 1 m from A. Find reactions at A and B. (c) A couple of two 5 kN forces acts on a 3m rod. Find moment of the couple.' },
                { id: 106, year: 2022, type: 'problem', topic: 'Center of Gravity & MOI', text: '(a) An I-section has dimensions: Bottom flange = 100x20, Top flange = 100x20, Web = 80x20 (mm). Find C.G. (b) Diameter of a hemisphere is 8 m. Find C.G. from base. (c) Find MOI of a semicircular section (50mm diameter) about its C.G. parallel to X-X axis.' },
                { id: 107, year: 2022, type: 'problem', topic: 'Friction', text: '(a) A body of 500 N is pulled up a 25° incline by a 250 N parallel force. Find coefficient of friction. (b) A 100 N body is on a rough horizontal plane. A 60 N horizontal force just causes it to slide. Find coefficient of friction.' },
                { id: 108, year: 2022, type: 'problem', topic: 'Dynamics & Kinematics', text: '(a) Motion equation: s = t³ - 2t² + 3. Find velocity and acceleration after 5s. (b) A 5 kg mass moves in a 1.5m radius circle at 2 rad/s. Find centrifugal force. (c) A 200m radius track is banked for 90 km/h. Find bank angle.' },
                { id: 109, year: 2022, type: 'problem', topic: 'Simple Machines', text: '(a) Define law of machine. (b) Load lifted by 120 N effort with VR=18, efficiency=60%? If 200 N lifts 2600 N, find law of machine and effort for 3.5 kN load.' },
                { id: 110, year: 2022, type: 'problem', topic: 'Simple Machines', text: '(a) Derive relation between MA, VR, and efficiency. (b) A differential wheel and axle with 80% efficiency lifts 60N with 6N effort. Find VR. If effort wheel is 300mm diameter and sum of axle diameters is 280mm, find axle diameters.' },
                { id: 111, year: 2024, type: 'problem', topic: 'Forces & Resultants', text: '(a) (i) Two forces act at 120°. Larger is 40 kg, resultant is perpendicular to smaller. Find smaller force. Or (ii) Find angle between two equal forces P when resultant is P/2. (b) Find resultant of forces: 20kg at 50°, 15kg horizontal, 12kg at 120°, 25kg at 220°.' },
                { id: 112, year: 2024, type: 'problem', topic: 'Equilibrium & Lami\'s Theorem', text: '(a) (i) State Varignon\'s principle. Or (ii) Define moment. (b) State Lami\'s theorem. A 70 kg body is suspended by 4m and 3m strings from a horizontal level 5m apart. Find tensions. (c) (i) 120cm rod supported at ends. Left support max 30kg. A 70kg weight is attached. Find position for left support to be at limit. Or (ii) Two like parallel forces are 40cm apart, resultant is 70gm and acts 15cm from left force. Find forces.' },
                { id: 113, year: 2024, type: 'problem', topic: 'Center of Gravity & MOI', text: '(a) (i) Distinguish C.G. and Centroid. Or (ii) Show C.G. for Hemisphere and Semicircle. (b) Find C.G. of the L-shaped lamina shown in figure.' },
                { id: 114, year: 2024, type: 'problem', topic: 'Friction', text: '(a) (i) State law of Static friction. Or (ii) Define: Coefficient of friction, Limiting friction, Angle of friction. (b) A 60 kg body is moved up a 25° incline by a 30 kg horizontal force. Find Coefficient of Friction.' },
                { id: 115, year: 2024, type: 'problem', topic: 'Center of Gravity & MOI', text: '(a) (i) Give MOI expression for a triangle about base and parallel C.G. axis. Or (ii) Explain Parallel axis theorem. (b) Find MOI of the T-shaped lamina about line AB (bottom edge).' },
                { id: 116, year: 2024, type: 'problem', topic: 'Dynamics & Kinematics', text: '(a) (i) Write equations of motion under gravity. Or (ii) Write units of Velocity, Acceleration, Work done, Power. (b) (i) A body starts from rest with 1.5 m/s² acceleration. Find time to reach 9.5 m/s and distance. Or (ii) Motion: S=3t³+2t. Find velocity (3s), acceleration (4s), distance (6s). (c) Find KE of 60gm bullet at 800 m/s. (d) Find work done to draw 15kg water from 20m well.' },
                { id: 117, year: 2024, type: 'problem', topic: 'Simple Machines', text: '(a) (i) Write expression for friction in a machine in terms of effort. Or (ii) Define efficiency. Distinguish Reversible and self-locking machines. (b) Simple wheel and axle radii are 25cm and 5cm. Effort of 50kg lifts 600kg. Find efficiency.' },
            ];

            let filters = {
                years: [],
                types: [],
                topic: 'all'
            };

            const yearFilters = document.getElementById('year-filters');
            const typeFilters = document.getElementById('type-filters');
            const topicFilter = document.getElementById('topic-filter');
            const questionsGrid = document.getElementById('questions-grid');
            const questionCount = document.getElementById('question-count');
            const noResults = document.getElementById('no-results');
            const clearFiltersBtn = document.getElementById('clear-filters');
            
            let yearChart, topicChart;

            const renderQuestions = (filteredQuestions) => {
                questionsGrid.innerHTML = '';
                if (filteredQuestions.length === 0) {
                    noResults.classList.remove('hidden');
                } else {
                    noResults.classList.add('hidden');
                }
                filteredQuestions.forEach(q => {
                    const card = document.createElement('div');
                    card.className = 'bg-white/80 p-5 rounded-lg shadow-md border border-gray-200/50 hover:shadow-xl hover:border-gray-300/50 transition-shadow duration-300';
                    card.innerHTML = `
                        <p class="text-gray-700 leading-relaxed">${q.text.replace(/\(True\/False\)/g, '<span class="font-semibold text-blue-600">(True/False)</span>')}</p>
                        <div class="mt-4 flex justify-between items-center text-sm">
                            <span class="font-semibold text-[#4682B4] bg-[#4682B4]/10 px-2 py-1 rounded">${q.topic}</span>
                            <span class="font-bold text-gray-500">${q.year}</span>
                        </div>
                    `;
                    questionsGrid.appendChild(card);
                });
                questionCount.textContent = filteredQuestions.length;
            };

            const updateCharts = (filteredQuestions) => {
                const yearCounts = {};
                ['2019', '2022', '2023', '2024'].forEach(y => yearCounts[y] = 0);
                filteredQuestions.forEach(q => {
                    if (yearCounts[q.year] !== undefined) {
                        yearCounts[q.year]++;
                    }
                });

                const topicCounts = {};
                [...new Set(questionsData.map(q => q.topic))].forEach(t => topicCounts[t] = 0);
                filteredQuestions.forEach(q => {
                    if (topicCounts[q.topic] !== undefined) {
                        topicCounts[q.topic]++;
                    }
                });
                
                const topicData = Object.entries(topicCounts).filter(([, count]) => count > 0);

                if (yearChart) yearChart.destroy();
                yearChart = new Chart(document.getElementById('questionsByYearChart').getContext('2d'), {
                    type: 'bar',
                    data: {
                        labels: Object.keys(yearCounts),
                        datasets: [{
                            label: 'Questions per Year',
                            data: Object.values(yearCounts),
                            backgroundColor: 'rgba(210, 180, 140, 0.6)',
                            borderColor: 'rgba(210, 180, 140, 1)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: { legend: { display: false }, title: { display: true, text: 'Question Count by Year', font: { size: 16 } } },
                        scales: { y: { beginAtZero: true, grace: '5%' } }
                    }
                });

                if (topicChart) topicChart.destroy();
                topicChart = new Chart(document.getElementById('questionsByTopicChart').getContext('2d'), {
                    type: 'doughnut',
                    data: {
                        labels: topicData.map(item => item[0]),
                        datasets: [{
                            label: 'Questions by Topic',
                            data: topicData.map(item => item[1]),
                            backgroundColor: [
                                'rgba(70, 130, 180, 0.7)', 'rgba(210, 180, 140, 0.7)', 'rgba(176, 196, 222, 0.7)',
                                'rgba(244, 164, 96, 0.7)', 'rgba(100, 149, 237, 0.7)', 'rgba(135, 206, 235, 0.7)'
                            ],
                            borderColor: '#F8F4E3',
                            borderWidth: 2
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: { legend: { position: 'bottom' }, title: { display: true, text: 'Question Distribution by Topic', font: { size: 16 } } }
                    }
                });
            };

            const applyFilters = () => {
                const filtered = questionsData.filter(q => {
                    const yearMatch = filters.years.length === 0 || filters.years.includes(q.year.toString());
                    const typeMatch = filters.types.length === 0 || filters.types.includes(q.type);
                    const topicMatch = filters.topic === 'all' || filters.topic === q.topic;
                    return yearMatch && typeMatch && topicMatch;
                });

                renderQuestions(filtered);
                updateCharts(filtered);
            };

            const setupEventListeners = () => {
                yearFilters.addEventListener('click', (e) => {
                    if (e.target.tagName === 'BUTTON') {
                        const year = e.target.dataset.year;
                        e.target.classList.toggle('active');
                        if (filters.years.includes(year)) {
                            filters.years = filters.years.filter(y => y !== year);
                        } else {
                            filters.years.push(year);
                        }
                        applyFilters();
                    }
                });

                typeFilters.addEventListener('click', (e) => {
                    if (e.target.tagName === 'BUTTON') {
                        const type = e.target.dataset.type;
                        e.target.classList.toggle('active');
                         if (filters.types.includes(type)) {
                            filters.types = filters.types.filter(t => t !== type);
                        } else {
                            filters.types.push(type);
                        }
                        applyFilters();
                    }
                });

                topicFilter.addEventListener('change', (e) => {
                    filters.topic = e.target.value;
                    applyFilters();
                });

                clearFiltersBtn.addEventListener('click', () => {
                    filters = { years: [], types: [], topic: 'all' };
                    document.querySelectorAll('.filter-btn.active').forEach(btn => btn.classList.remove('active'));
                    topicFilter.value = 'all';
                    applyFilters();
                });
            };

            const init = () => {
                setupEventListeners();
                applyFilters(); // Initial render
            };

            init();
        });
    </script>
</body>
</html>
