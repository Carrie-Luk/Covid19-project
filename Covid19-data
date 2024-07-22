-- SELECT * FROM Porfolioproject.covidvaccinations
-- order by 3,4;

SELECT * 
FROM Porfolioproject.coviddeaths
WHERE continent > ''
order by 3,4;

-- select data that we will be using

SELECT location, date, total_cases, new_cases, total_deaths, population
FROM Porfolioproject.coviddeaths
WHERE continent > ''
ORDER BY 1,2;  

-- looking at total cases vs total deaths in Canada

SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
FROM Porfolioproject.coviddeaths
WHERE location LIKE '%canada%'
ORDER BY 1,2; 

-- looking at total cases vs population

SELECT location, date, total_cases, population, (total_cases/population)*100 as CasePercentage
FROM Porfolioproject.coviddeaths
WHERE location LIKE '%canada%'
ORDER BY 1,2; 

-- looking at countries with highest infection rates compared to population

SELECT location, population, MAX(total_cases) as HighestInfectionCount, 
MAX((total_cases/population))*100 as PercentPopulationInfected
FROM Porfolioproject.coviddeaths
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC;

-- looking at countries with highest death count per population

SELECT location, MAX(cast(total_deaths as float)) as HighestDeathCount
FROM Porfolioproject.coviddeaths
WHERE continent > ''
GROUP BY location
ORDER BY HighestDeathCount DESC;

-- looking at a break down by continent
-- showing the continents with the highest death count

SELECT continent, MAX(cast(total_deaths as float)) as HighestDeathCount
FROM Porfolioproject.coviddeaths
WHERE continent > ''
GROUP BY continent
ORDER BY HighestDeathCount DESC;

-- global numbers

SELECT date, SUM(new_cases) as total_cases,SUM(cast(new_deaths as float)) as total_deaths,
SUM(cast(new_deaths as float))/SUM(new_cases)*100 as DeathPercentage
FROM Porfolioproject.coviddeaths
-- WHERE location LIKE '%canada%' 
WHERE continent > ''
GROUP BY date
ORDER BY 1,2; 


-- looking at total population vs vaccinations 
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(cast(vac.new_vaccinations as float)) OVER (Partition by dea.location ORDER BY dea.location, dea.Date)
AS RollingPeopleVaccinated
-- , (RollingPeopleVaccinated/population)*100
FROM Porfolioproject.coviddeaths as dea 
JOIN Porfolioproject.covidvaccinations as vac 
	ON dea.location = vac.location
	AND dea.date = vac.date
WHERE dea.continent > ''
-- WHERE dea.location LIKE '%canada%' 
ORDER BY 2,3;


-- Use CTE 
WITH PopvsVac (continent, location, date, population, New_Vaccinations, RollingPeopleVaccinated)
as 
(
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(cast(vac.new_vaccinations as float)) OVER (Partition by dea.location ORDER BY dea.location, dea.Date)
AS RollingPeopleVaccinated
-- , (RollingPeopleVaccinated/population)*100
FROM Porfolioproject.coviddeaths as dea 
JOIN Porfolioproject.covidvaccinations as vac 
	ON dea.location = vac.location
	AND dea.date = vac.date
WHERE dea.continent > ''
-- WHERE dea.location LIKE '%canada%' 
-- ORDER BY 2,3
)
Select *, (RollingPeopleVaccinated/Population)*100
From PopvsVac; 

-- Temp Table

DROP TABLE IF exists #PercentPopulationVaccinated

CREATE TABLE #PercentPopulationVaccinated
(
Continent varchar(255),
Location varchar(255),
Date datetime,
Population int,
New_vaccinations int,
RollingPeopleVaccinated int
);

Insert Into #PercentPopulationVaccinated

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(cast(vac.new_vaccinations as float)) OVER (Partition by dea.location ORDER BY dea.location, dea.Date)
AS RollingPeopleVaccinated
-- , (RollingPeopleVaccinated/population)*100
FROM Porfolioproject.coviddeaths as dea 
JOIN Porfolioproject.covidvaccinations as vac 
	ON dea.location = vac.location
	AND dea.date = vac.date

SELECT *, (RollingPeopleVaccinated/Population)*100
From #PercentPopulationVaccinated 

-- Creating View to store data for later visualizations 

Create view PercentPopulationVaccinated as
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(cast(vac.new_vaccinations as float)) OVER (Partition by dea.location ORDER BY dea.location, dea.Date)
AS RollingPeopleVaccinated
-- , (RollingPeopleVaccinated/population)*100
FROM Porfolioproject.coviddeaths as dea 
JOIN Porfolioproject.covidvaccinations as vac 
	ON dea.location = vac.location
	AND dea.date = vac.date
WHERE dea.continent > ''; 
-- ORDER BY 2,3

SELECT * 
FROM PercentPopulationVaccinated ;
