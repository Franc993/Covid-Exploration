Select *
From PorfolioProject1..CovidDeaths
Where continent is not null
order by 3,4


--Select *
--From PorfolioProject1..CovidVaccinations
--order by 3,4


--Select Data that we are going to be using
Select Location, date, total_cases, new_cases, total_deaths, population
From PorfolioProject1..CovidDeaths
Where continent is not null
order by 1,2

--Looking at Total Cases vs Total Deaths
--Shows likelihood of dying if you contract covid in your country

Select Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
From PorfolioProject1..CovidDeaths
Where location like '%united kingdom%'
and continent is not null
order by 1,2



-- Looking at Total Cases vs Population
-- Shows what percentage of population got Covid

Select Location, date, Population, total_cases,(total_cases/population)*100 as PercentPopulationInfected
From PorfolioProject1..CovidDeaths
Where location like '%united kingdom%'
and continent is not null
order by 1,2


-- Looking at Countries with Highest Infection Rate compared to Population

Select Location, Population, MAX(total_cases) as HighestInfectionCount, MAX((total_cases/population))*100 as PercentPopulationInfected
From PorfolioProject1..CovidDeaths
--Where location like '%united kingdom%'
Where continent is not null
Group by location,population
order by PercentPopulationInfected desc


-- Showing Countries with Highest Death Count per Population
-- Data type is wrong so we cast it as an INT


Select Location, Max(cast(total_deaths as int)) as TotalDeathCount
From PorfolioProject1..CovidDeaths
--Where location like '%united kingdom%'
Where continent is not null
Group by location
order by TotalDeathCount desc

--LET'S BREAK THINGS DOWN BY CONTINENT

--Showing continents with the highest death count per population

Select continent, Max(cast(total_deaths as int)) as TotalDeathCount
From PorfolioProject1..CovidDeaths
--Where location like '%united kingdom%'
Where continent is not null
Group by continent
order by TotalDeathCount desc



-- GLOBAL NUMBERS
--We need to do an aggregate function

Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(new_cases)*100 as DeathPercentage
From PorfolioProject1..CovidDeaths
--Where location like '%united kingdom%'
where continent is not null
--Group by date
order by 1,2

-- Looking at Total Population vs Vaccinations
--USE CTE
With PopvsVac (Continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated)
as
(
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM (CONVERT(bigint, vac.new_vaccinations)) OVER (Partition by dea.location Order by dea.location,
dea.Date) as RollingPeopleVaccinated
--, (RollingPeopleVaccinated/population)*100
From PorfolioProject1..CovidDeaths dea
Join PorfolioProject1..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
--order by 2,3
)
Select *, (RollingPeopleVaccinated/Population)*100
From PopvsVac


--TEMP TABLE
DROP table if exists #PercentPopulationVaccinated
Create Table #PercentPopulationVaccinated
(
Continent nvarchar (255),
Location nvarchar (255),
Date datetime,
Population numeric,
New_vaccinations numeric,
RollingPeopleVaccinated numeric
)

Insert into #PercentPopulationVaccinated
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM (CONVERT(bigint, vac.new_vaccinations)) OVER (Partition by dea.location Order by dea.location,
dea.Date) as RollingPeopleVaccinated
--, (RollingPeopleVaccinated/population)*100
From PorfolioProject1..CovidDeaths dea
Join PorfolioProject1..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
--where dea.continent is not null
--order by 2,3

Select *, (RollingPeopleVaccinated/Population)*100
From #PercentPopulationVaccinated



--Creating View to store data for later visualizations

Create View PercentPopulationVaccinated as
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM (CONVERT(bigint, vac.new_vaccinations)) OVER (Partition by dea.location Order by dea.location,
dea.Date) as RollingPeopleVaccinated
--, (RollingPeopleVaccinated/population)*100
From PorfolioProject1..CovidDeaths dea
Join PorfolioProject1..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
--order by 2,3


Select *
From PercentPopulationVaccinated
