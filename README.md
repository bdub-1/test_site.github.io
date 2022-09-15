# test_site.github.io
{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "1260d102",
   "metadata": {},
   "source": [
    "# Atlanta Crime Report: 2009 - 2022"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f52ecd12",
   "metadata": {},
   "source": [
    "### Import Libraries"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "9b65ae72",
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import altair as alt\n",
    "import os\n",
    "from csv_parser import csv_columnNames_to_rows\n",
    "import datetime"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "d4c262e1",
   "metadata": {},
   "source": [
    "### Specify target folder"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "6a78b769",
   "metadata": {},
   "outputs": [],
   "source": [
    "dirname = 'COBRA-Data'"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "083b8d54",
   "metadata": {},
   "source": [
    "### View column names of each dataFrame\n",
    "Wrote a script to create a dataFrame out of the column names for each CSV file in a given folder.  \n",
    "https://www.linkedin.com/pulse/csv-column-name-parser-brandon-wilson/"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "64102e5f",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Importing: ['COBRA-2009-2019.csv', 'COBRA-2020(NEW RMS 9-30 12-31).csv', 'COBRA-2020-OldRMS-09292020.csv', 'COBRA-2021.csv', 'COBRA-2022.csv'] \n",
      "...\n",
      "loading: COBRA-Data/COBRA-2009-2019.csv\n",
      "loading: COBRA-Data/COBRA-2020(NEW RMS 9-30 12-31).csv\n",
      "loading: COBRA-Data/COBRA-2020-OldRMS-09292020.csv\n",
      "loading: COBRA-Data/COBRA-2021.csv\n",
      "loading: COBRA-Data/COBRA-2022.csv\n",
      "... \n",
      "Success\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>COBRA-2009-2019.csv</th>\n",
       "      <th>COBRA-2020(NEW RMS 9-30 12-31).csv</th>\n",
       "      <th>COBRA-2020-OldRMS-09292020.csv</th>\n",
       "      <th>COBRA-2021.csv</th>\n",
       "      <th>COBRA-2022.csv</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Report Number</td>\n",
       "      <td>offense_id</td>\n",
       "      <td>offense_id</td>\n",
       "      <td>offense_id</td>\n",
       "      <td>offense_id</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Report Date</td>\n",
       "      <td>rpt_date</td>\n",
       "      <td>rpt_date</td>\n",
       "      <td>rpt_date</td>\n",
       "      <td>rpt_date</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Occur Date</td>\n",
       "      <td>occur_date</td>\n",
       "      <td>occur_date</td>\n",
       "      <td>occur_date</td>\n",
       "      <td>occur_date</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Occur Time</td>\n",
       "      <td>occur_time</td>\n",
       "      <td>occur_time</td>\n",
       "      <td>occur_day</td>\n",
       "      <td>occur_day</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Possible Date</td>\n",
       "      <td>poss_date</td>\n",
       "      <td>poss_date</td>\n",
       "      <td>occur_day_num</td>\n",
       "      <td>occur_day_num</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Possible Time</td>\n",
       "      <td>poss_time</td>\n",
       "      <td>poss_time</td>\n",
       "      <td>occur_time</td>\n",
       "      <td>occur_time</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Beat</td>\n",
       "      <td>beat</td>\n",
       "      <td>beat</td>\n",
       "      <td>poss_date</td>\n",
       "      <td>poss_date</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Apartment Office Prefix</td>\n",
       "      <td>apt_office_prefix</td>\n",
       "      <td>apartment_office_prefix</td>\n",
       "      <td>poss_time</td>\n",
       "      <td>poss_time</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>Apartment Number</td>\n",
       "      <td>apt_office_num</td>\n",
       "      <td>apartment_number</td>\n",
       "      <td>beat</td>\n",
       "      <td>beat</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>Location</td>\n",
       "      <td>location</td>\n",
       "      <td>location</td>\n",
       "      <td>zone</td>\n",
       "      <td>zone</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>Shift Occurence</td>\n",
       "      <td>MinOfucr</td>\n",
       "      <td>watch</td>\n",
       "      <td>location</td>\n",
       "      <td>location</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>Location Type</td>\n",
       "      <td>dispo_code</td>\n",
       "      <td>location_type</td>\n",
       "      <td>ibr_code</td>\n",
       "      <td>ibr_code</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>UCR Literal</td>\n",
       "      <td>Shift</td>\n",
       "      <td>UC2_Literal</td>\n",
       "      <td>UC2_Literal</td>\n",
       "      <td>UC2_Literal</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>UCR #</td>\n",
       "      <td>loc_type</td>\n",
       "      <td>UCR_Number</td>\n",
       "      <td>neighborhood</td>\n",
       "      <td>neighborhood</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>IBR Code</td>\n",
       "      <td>UC2_Literal</td>\n",
       "      <td>neighborhood</td>\n",
       "      <td>npu</td>\n",
       "      <td>npu</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>15</th>\n",
       "      <td>Neighborhood</td>\n",
       "      <td>ibr_code</td>\n",
       "      <td>npu</td>\n",
       "      <td>lat</td>\n",
       "      <td>lat</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>NPU</td>\n",
       "      <td>neighborhood</td>\n",
       "      <td>lat</td>\n",
       "      <td>long</td>\n",
       "      <td>long</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>17</th>\n",
       "      <td>Latitude</td>\n",
       "      <td>npu</td>\n",
       "      <td>long</td>\n",
       "      <td>XXX</td>\n",
       "      <td>XXX</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18</th>\n",
       "      <td>Longitude</td>\n",
       "      <td>long</td>\n",
       "      <td>XXX</td>\n",
       "      <td>XXX</td>\n",
       "      <td>XXX</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19</th>\n",
       "      <td>XXX</td>\n",
       "      <td>lat</td>\n",
       "      <td>XXX</td>\n",
       "      <td>XXX</td>\n",
       "      <td>XXX</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "        COBRA-2009-2019.csv COBRA-2020(NEW RMS 9-30 12-31).csv  \\\n",
       "0             Report Number                         offense_id   \n",
       "1               Report Date                           rpt_date   \n",
       "2                Occur Date                         occur_date   \n",
       "3                Occur Time                         occur_time   \n",
       "4             Possible Date                          poss_date   \n",
       "5             Possible Time                          poss_time   \n",
       "6                      Beat                               beat   \n",
       "7   Apartment Office Prefix                  apt_office_prefix   \n",
       "8          Apartment Number                     apt_office_num   \n",
       "9                  Location                           location   \n",
       "10          Shift Occurence                           MinOfucr   \n",
       "11            Location Type                         dispo_code   \n",
       "12              UCR Literal                              Shift   \n",
       "13                    UCR #                           loc_type   \n",
       "14                 IBR Code                        UC2_Literal   \n",
       "15             Neighborhood                           ibr_code   \n",
       "16                      NPU                       neighborhood   \n",
       "17                 Latitude                                npu   \n",
       "18                Longitude                               long   \n",
       "19                      XXX                                lat   \n",
       "\n",
       "   COBRA-2020-OldRMS-09292020.csv COBRA-2021.csv COBRA-2022.csv  \n",
       "0                      offense_id     offense_id     offense_id  \n",
       "1                        rpt_date       rpt_date       rpt_date  \n",
       "2                      occur_date     occur_date     occur_date  \n",
       "3                      occur_time      occur_day      occur_day  \n",
       "4                       poss_date  occur_day_num  occur_day_num  \n",
       "5                       poss_time     occur_time     occur_time  \n",
       "6                            beat      poss_date      poss_date  \n",
       "7         apartment_office_prefix      poss_time      poss_time  \n",
       "8                apartment_number           beat           beat  \n",
       "9                        location           zone           zone  \n",
       "10                          watch       location       location  \n",
       "11                  location_type       ibr_code       ibr_code  \n",
       "12                    UC2_Literal    UC2_Literal    UC2_Literal  \n",
       "13                     UCR_Number   neighborhood   neighborhood  \n",
       "14                   neighborhood            npu            npu  \n",
       "15                            npu            lat            lat  \n",
       "16                            lat           long           long  \n",
       "17                           long            XXX            XXX  \n",
       "18                            XXX            XXX            XXX  \n",
       "19                            XXX            XXX            XXX  "
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "csv_columnNames_to_rows(dirname)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "d73ebf84",
   "metadata": {},
   "source": [
    "## Prepare data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "12aa9e63",
   "metadata": {},
   "outputs": [],
   "source": [
    "dfs = []\n",
    "c = 0\n",
    "for file in os.listdir(dirname):\n",
    "    if c == 0:\n",
    "        dfs.append(pd.read_csv(dirname + '/' + file, low_memory=False, parse_dates=['Occur Date']))\n",
    "    else:\n",
    "        dfs.append(pd.read_csv(dirname + '/' + file, low_memory=False, parse_dates=['occur_date']))\n",
    "    c+=1"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "8bad6db8",
   "metadata": {},
   "source": [
    "### Keep and rename columns for merge"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "60577563",
   "metadata": {
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "keep = ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n",
    "\n",
    "dfs[1].rename(columns = {'offense_id':'Report Number', 'rpt_date':'Report Date', 'occur_date':'Occur Date', \n",
    "                         'occur_time':'Occur Time', 'location':'Location', 'UC2_Literal':'UCR Literal', \n",
    "                          'neighborhood':'Neighborhood', 'lat':'Latitude', 'long':'Longitude'}, inplace = True)\n",
    "\n",
    "dfs[2].rename(columns = {'offense_id':'Report Number', 'rpt_date':'Report Date', 'occur_date':'Occur Date', \n",
    "                         'occur_time':'Occur Time', 'location':'Location', 'UC2_Literal':'UCR Literal', \n",
    "                          'neighborhood':'Neighborhood', 'lat':'Latitude', 'long':'Longitude'}, inplace = True)\n",
    "\n",
    "dfs[3].rename(columns = {'offense_id':'Report Number', 'rpt_date':'Report Date', 'occur_date':'Occur Date', \n",
    "                         'occur_time':'Occur Time', 'location':'Location', 'UC2_Literal':'UCR Literal', \n",
    "                          'neighborhood':'Neighborhood', 'lat':'Latitude', 'long':'Longitude'}, inplace = True)\n",
    "\n",
    "dfs[4].rename(columns = {'offense_id':'Report Number', 'rpt_date':'Report Date', 'occur_date':'Occur Date', \n",
    "                         'occur_time':'Occur Time', 'location':'Location', 'UC2_Literal':'UCR Literal', \n",
    "                          'neighborhood':'Neighborhood', 'lat':'Latitude', 'long':'Longitude'}, inplace = True)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "083421d8",
   "metadata": {},
   "source": [
    "### Drop unwanted columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "464f4835",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Passes in a list data frames and a list of column names to keep for each data frame\n",
    "def dfs_dropcols(dfs, keep):\n",
    "    c = 0\n",
    "    for df in dfs:\n",
    "        dfs[c] = df[keep]\n",
    "        c+=1\n",
    "    return dfs\n",
    "dfs = dfs_dropcols(dfs, keep)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "id": "c1606b18",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "df: ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n",
      "df: ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n",
      "df: ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n",
      "df: ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n",
      "df: ['Report Number', 'Occur Date', 'Occur Time', 'Location', 'UCR Literal', 'Neighborhood', 'Latitude', 'Longitude']\n"
     ]
    }
   ],
   "source": [
    "# Test drop function\n",
    "for df in dfs:\n",
    "    print('df:', list(df))"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "00a7bf1e",
   "metadata": {},
   "source": [
    "### Fix Date Formatting and Range"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "02c70dd6",
   "metadata": {
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "# Checking date formatting\n",
    "# for df in dfs:\n",
    "#     print(df['Occur Date'].head())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "id": "6e95b8db",
   "metadata": {
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "dfs[1]['Occur Date'] = pd.to_datetime(dfs[1]['Occur Date'], errors = 'coerce')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "f8aa9594",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Checking date formatting after adjustment\n",
    "# for df in dfs:\n",
    "#     print(df['Occur Date'].head())"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f4456109",
   "metadata": {},
   "source": [
    "### Fix Time Formatting"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "305e3d08",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Checking time formatting\n",
    "# for df in dfs:\n",
    "#     print(df['Occur Time'].head())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "c432f716",
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0         1145\n",
       "1         1330\n",
       "2         1500\n",
       "3         1450\n",
       "4         1600\n",
       "          ... \n",
       "342909    2030\n",
       "342910    0432\n",
       "342911    0920\n",
       "342912    1853\n",
       "342913    2045\n",
       "Name: Occur Time, Length: 342914, dtype: object"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# fix dfs[0]\n",
    "dfs[0]['Occur Time']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "978f727f",
   "metadata": {
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "lst = []\n",
    "for time in dfs[0]['Occur Time']:\n",
    "    lst.append(time[0:2]+':'+time[2:])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "f272db0f",
   "metadata": {},
   "outputs": [],
   "source": [
    "dfs[0]['Occur Time'] = lst"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "536deca2",
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Report Number</th>\n",
       "      <th>Occur Date</th>\n",
       "      <th>Occur Time</th>\n",
       "      <th>Location</th>\n",
       "      <th>UCR Literal</th>\n",
       "      <th>Neighborhood</th>\n",
       "      <th>Latitude</th>\n",
       "      <th>Longitude</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>90010930</td>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>11:45</td>\n",
       "      <td>2841 GREENBRIAR PKWY</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Greenbriar</td>\n",
       "      <td>33.68845</td>\n",
       "      <td>-84.49328</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>90011083</td>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>13:30</td>\n",
       "      <td>12 BROAD ST SW</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Downtown</td>\n",
       "      <td>33.75320</td>\n",
       "      <td>-84.39201</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>90011208</td>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>15:00</td>\n",
       "      <td>3500 MARTIN L KING JR DR SW</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Adamsville</td>\n",
       "      <td>33.75735</td>\n",
       "      <td>-84.50282</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>90011218</td>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>14:50</td>\n",
       "      <td>3393 PEACHTREE RD NE</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Lenox</td>\n",
       "      <td>33.84676</td>\n",
       "      <td>-84.36212</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>90011289</td>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>16:00</td>\n",
       "      <td>2841 GREENBRIAR PKWY SW</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Greenbriar</td>\n",
       "      <td>33.68677</td>\n",
       "      <td>-84.49773</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Report Number Occur Date Occur Time                     Location  \\\n",
       "0       90010930 2009-01-01      11:45         2841 GREENBRIAR PKWY   \n",
       "1       90011083 2009-01-01      13:30               12 BROAD ST SW   \n",
       "2       90011208 2009-01-01      15:00  3500 MARTIN L KING JR DR SW   \n",
       "3       90011218 2009-01-01      14:50         3393 PEACHTREE RD NE   \n",
       "4       90011289 2009-01-01      16:00      2841 GREENBRIAR PKWY SW   \n",
       "\n",
       "           UCR Literal Neighborhood  Latitude  Longitude  \n",
       "0  LARCENY-NON VEHICLE   Greenbriar  33.68845  -84.49328  \n",
       "1  LARCENY-NON VEHICLE     Downtown  33.75320  -84.39201  \n",
       "2  LARCENY-NON VEHICLE   Adamsville  33.75735  -84.50282  \n",
       "3  LARCENY-NON VEHICLE        Lenox  33.84676  -84.36212  \n",
       "4  LARCENY-NON VEHICLE   Greenbriar  33.68677  -84.49773  "
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "dfs[0].head()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "b63e1665",
   "metadata": {},
   "source": [
    "## Union dfs tables"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "id": "1c2a217c",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "342914 rows in dfs[0]\n",
      "7249 rows in dfs[1]\n",
      "14831 rows in dfs[2]\n",
      "21397 rows in dfs[3]\n",
      "14605 rows in dfs[4]\n",
      "...\n",
      "400996 rows in dfs total\n",
      "400813 rows in df_union total\n",
      "...\n",
      "183 duplicates were removed\n"
     ]
    }
   ],
   "source": [
    "df_union = None\n",
    "for df in dfs:\n",
    "    df_union = pd.concat([df_union, df]).drop_duplicates()\n",
    "    \n",
    "# check union \n",
    "summ = 0 \n",
    "c = 0\n",
    "for df in dfs:\n",
    "    print(df.shape[0], f'rows in dfs[{c}]')\n",
    "    summ+=df.shape[0]\n",
    "    c+=1\n",
    "print('...')\n",
    "print(summ, 'rows in dfs total')\n",
    "print(df_union.shape[0], 'rows in df_union total')\n",
    "print('...')\n",
    "print(summ - df_union.shape[0], 'duplicates were removed')"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "fe18083b",
   "metadata": {},
   "source": [
    "### Processing dates "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "09b8a95b",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Report Number</th>\n",
       "      <th>Occur Date</th>\n",
       "      <th>Occur Time</th>\n",
       "      <th>Location</th>\n",
       "      <th>UCR Literal</th>\n",
       "      <th>Neighborhood</th>\n",
       "      <th>Latitude</th>\n",
       "      <th>Longitude</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>240079</th>\n",
       "      <td>160442700</td>\n",
       "      <td>1916-01-07</td>\n",
       "      <td>12:15</td>\n",
       "      <td>710 JEWEL CT SW</td>\n",
       "      <td>BURGLARY-RESIDENCE</td>\n",
       "      <td>Midwest Cascade</td>\n",
       "      <td>33.725830</td>\n",
       "      <td>-84.550500</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>243237</th>\n",
       "      <td>160902737</td>\n",
       "      <td>1916-03-29</td>\n",
       "      <td>23:00</td>\n",
       "      <td>180 WALKER ST SW</td>\n",
       "      <td>AUTO THEFT</td>\n",
       "      <td>Castleberry Hill</td>\n",
       "      <td>33.749640</td>\n",
       "      <td>-84.401320</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>243631</th>\n",
       "      <td>160951996</td>\n",
       "      <td>1916-04-02</td>\n",
       "      <td>17:00</td>\n",
       "      <td>2175 PIEDMONT RD NE</td>\n",
       "      <td>BURGLARY-NONRES</td>\n",
       "      <td>Lindridge/Martin Manor</td>\n",
       "      <td>33.817160</td>\n",
       "      <td>-84.366320</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>247606</th>\n",
       "      <td>161460989</td>\n",
       "      <td>1916-05-15</td>\n",
       "      <td>20:00</td>\n",
       "      <td>102 W PACES FERRY RD NW</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Peachtree Heights West</td>\n",
       "      <td>33.841310</td>\n",
       "      <td>-84.384270</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>248620</th>\n",
       "      <td>161592043</td>\n",
       "      <td>1916-05-30</td>\n",
       "      <td>14:00</td>\n",
       "      <td>4666 EDWINA LN SW</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Greenbriar Village</td>\n",
       "      <td>33.703090</td>\n",
       "      <td>-84.540440</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19172</th>\n",
       "      <td>213303737</td>\n",
       "      <td>NaT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>2265 MARIETTA BLVD NW\\nATLANTA, GA 30318\\nUNIT...</td>\n",
       "      <td>AUTO THEFT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>33.818629</td>\n",
       "      <td>-84.449292</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>20547</th>\n",
       "      <td>213490677</td>\n",
       "      <td>NaT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>507 BISHOP ST NW\\nATL, GA 30318\\nUNITED STATES</td>\n",
       "      <td>LARCENY-FROM VEHICLE</td>\n",
       "      <td>Loring Heights</td>\n",
       "      <td>33.792566</td>\n",
       "      <td>-84.404601</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9837</th>\n",
       "      <td>221720624</td>\n",
       "      <td>NaT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>1823 JONESBORO RD SE\\nATLANTA, GA 30315\\nUNITE...</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Lakewood Heights</td>\n",
       "      <td>33.704411</td>\n",
       "      <td>-84.378159</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12347</th>\n",
       "      <td>222081763</td>\n",
       "      <td>NaT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>1348 BENTEEN WAY SE\\nATL, GA 30315\\nUNITED STATES</td>\n",
       "      <td>LARCENY-NON VEHICLE</td>\n",
       "      <td>Benteen Park</td>\n",
       "      <td>33.718693</td>\n",
       "      <td>-84.366332</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14586</th>\n",
       "      <td>222431947</td>\n",
       "      <td>NaT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>565 HANK AARON DR SW\\nATL, GA 30312\\nUNITED ST...</td>\n",
       "      <td>AGG ASSAULT</td>\n",
       "      <td>Summerhill</td>\n",
       "      <td>33.737698</td>\n",
       "      <td>-84.389983</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>400813 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "        Report Number Occur Date Occur Time  \\\n",
       "240079      160442700 1916-01-07      12:15   \n",
       "243237      160902737 1916-03-29      23:00   \n",
       "243631      160951996 1916-04-02      17:00   \n",
       "247606      161460989 1916-05-15      20:00   \n",
       "248620      161592043 1916-05-30      14:00   \n",
       "...               ...        ...        ...   \n",
       "19172       213303737        NaT        NaN   \n",
       "20547       213490677        NaT        NaN   \n",
       "9837        221720624        NaT        NaN   \n",
       "12347       222081763        NaT        NaN   \n",
       "14586       222431947        NaT        NaN   \n",
       "\n",
       "                                                 Location  \\\n",
       "240079                                    710 JEWEL CT SW   \n",
       "243237                                   180 WALKER ST SW   \n",
       "243631                                2175 PIEDMONT RD NE   \n",
       "247606                            102 W PACES FERRY RD NW   \n",
       "248620                                  4666 EDWINA LN SW   \n",
       "...                                                   ...   \n",
       "19172   2265 MARIETTA BLVD NW\\nATLANTA, GA 30318\\nUNIT...   \n",
       "20547      507 BISHOP ST NW\\nATL, GA 30318\\nUNITED STATES   \n",
       "9837    1823 JONESBORO RD SE\\nATLANTA, GA 30315\\nUNITE...   \n",
       "12347   1348 BENTEEN WAY SE\\nATL, GA 30315\\nUNITED STATES   \n",
       "14586   565 HANK AARON DR SW\\nATL, GA 30312\\nUNITED ST...   \n",
       "\n",
       "                 UCR Literal            Neighborhood   Latitude  Longitude  \n",
       "240079    BURGLARY-RESIDENCE         Midwest Cascade  33.725830 -84.550500  \n",
       "243237            AUTO THEFT        Castleberry Hill  33.749640 -84.401320  \n",
       "243631       BURGLARY-NONRES  Lindridge/Martin Manor  33.817160 -84.366320  \n",
       "247606   LARCENY-NON VEHICLE  Peachtree Heights West  33.841310 -84.384270  \n",
       "248620   LARCENY-NON VEHICLE      Greenbriar Village  33.703090 -84.540440  \n",
       "...                      ...                     ...        ...        ...  \n",
       "19172             AUTO THEFT                     NaN  33.818629 -84.449292  \n",
       "20547   LARCENY-FROM VEHICLE          Loring Heights  33.792566 -84.404601  \n",
       "9837     LARCENY-NON VEHICLE        Lakewood Heights  33.704411 -84.378159  \n",
       "12347    LARCENY-NON VEHICLE            Benteen Park  33.718693 -84.366332  \n",
       "14586            AGG ASSAULT              Summerhill  33.737698 -84.389983  \n",
       "\n",
       "[400813 rows x 8 columns]"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_union.sort_values(by = 'Occur Date', ascending = True, inplace = True)  \n",
    "df_union"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "6edc7db9",
   "metadata": {},
   "source": [
    "__________________________________"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f7b204a8",
   "metadata": {},
   "source": [
    "# Analysis"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "221c9016",
   "metadata": {},
   "source": [
    "### Time-series analysis\n",
    "This is a time-series analysis of volume."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "d4be24f0",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Occur Date</th>\n",
       "      <th>Volume</th>\n",
       "      <th>90-day</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>2009-01-01</td>\n",
       "      <td>133</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2009-01-02</td>\n",
       "      <td>144</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>2009-01-03</td>\n",
       "      <td>126</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>2009-01-04</td>\n",
       "      <td>95</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>2009-01-05</td>\n",
       "      <td>126</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4987</th>\n",
       "      <td>2022-08-28</td>\n",
       "      <td>54</td>\n",
       "      <td>64.311111</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4988</th>\n",
       "      <td>2022-08-29</td>\n",
       "      <td>65</td>\n",
       "      <td>64.322222</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4989</th>\n",
       "      <td>2022-08-30</td>\n",
       "      <td>40</td>\n",
       "      <td>64.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4990</th>\n",
       "      <td>2022-08-31</td>\n",
       "      <td>49</td>\n",
       "      <td>63.722222</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4991</th>\n",
       "      <td>2022-09-01</td>\n",
       "      <td>6</td>\n",
       "      <td>63.011111</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>4992 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     Occur Date  Volume     90-day\n",
       "0    2009-01-01     133        NaN\n",
       "1    2009-01-02     144        NaN\n",
       "2    2009-01-03     126        NaN\n",
       "3    2009-01-04      95        NaN\n",
       "4    2009-01-05     126        NaN\n",
       "...         ...     ...        ...\n",
       "4987 2022-08-28      54  64.311111\n",
       "4988 2022-08-29      65  64.322222\n",
       "4989 2022-08-30      40  64.000000\n",
       "4990 2022-08-31      49  63.722222\n",
       "4991 2022-09-01       6  63.011111\n",
       "\n",
       "[4992 rows x 3 columns]"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Select columns of interest\n",
    "keep = ['Report Number', 'Occur Date']\n",
    "ts_data = df_union[keep]\n",
    "\n",
    "# Drop rows with NaT and older than 2009\n",
    "ts_data = ts_data.dropna()\n",
    "ts_data = ts_data.drop(ts_data[ts_data['Occur Date'] < '2009-01-01'].index)\n",
    "seasonal_data = ts_data # creating copy of data here to use in another model\n",
    "\n",
    "# Group by date to track volume each day\n",
    "ts_data = ts_data.groupby('Occur Date')['Report Number'].count().reset_index() \n",
    "ts_data.columns = ['Occur Date', 'Volume']\n",
    "\n",
    "# Add a 90-day moving average\n",
    "ts_data['90-day'] = ts_data.rolling(window = 90).mean()\n",
    "\n",
    "ts_data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "95a837cf",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Build the time-series model\n",
    "\n",
    "a = alt.Chart(ts_data).mark_area().encode(\n",
    "    x = 'Occur Date',\n",
    "    y = 'Volume',\n",
    "    color = alt.value('#7CB9E8')\n",
    ").properties(\n",
    "    width = 800,\n",
    "   height = 300,\n",
    "    title = 'Crime Volume Over Time'\n",
    ").interactive()\n",
    "\n",
    "r = alt.Chart(ts_data).mark_line().encode(\n",
    "    x = 'Occur Date',\n",
    "    y = '90-day',\n",
    "    color = alt.value('#FF0808'),\n",
    "    tooltip = ('Volume')\n",
    ")\n",
    "\n",
    "time = alt.layer(a, r)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "b3b4e161",
   "metadata": {},
   "source": [
    "### Seasonal Analysis"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "ef72b75c",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Month</th>\n",
       "      <th>Volume</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Jan</td>\n",
       "      <td>33244</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Feb</td>\n",
       "      <td>26939</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Mar</td>\n",
       "      <td>29780</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Apr</td>\n",
       "      <td>31384</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>May</td>\n",
       "      <td>35001</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Jun</td>\n",
       "      <td>35301</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Jul</td>\n",
       "      <td>37203</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Aug</td>\n",
       "      <td>36348</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>Sep</td>\n",
       "      <td>33019</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>Oct</td>\n",
       "      <td>34325</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>Nov</td>\n",
       "      <td>32812</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>Dec</td>\n",
       "      <td>33772</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Month  Volume\n",
       "0    Jan   33244\n",
       "1    Feb   26939\n",
       "2    Mar   29780\n",
       "3    Apr   31384\n",
       "4    May   35001\n",
       "5    Jun   35301\n",
       "6    Jul   37203\n",
       "7    Aug   36348\n",
       "8    Sep   33019\n",
       "9    Oct   34325\n",
       "10   Nov   32812\n",
       "11   Dec   33772"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Renaming columns\n",
    "seasonal_data.rename(columns = {'Occur Date':'Month', 'Report Number':'Volume'}, inplace=True)\n",
    "\n",
    "# Group by date to track volume each day\n",
    "seasonal_data = seasonal_data.groupby(seasonal_data['Month'].dt.month)['Volume'].count().reset_index()\n",
    "\n",
    "#Converting into month name (I did this extra step to avoid weird sorting from group by straight into month name)\n",
    "seasonal_data['Month'] = pd.to_datetime(seasonal_data['Month'], format='%m').dt.month_name().str.slice(stop=3)\n",
    "\n",
    "seasonal_data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "415165e6",
   "metadata": {},
   "outputs": [],
   "source": [
    "seasonal = alt.Chart(seasonal_data).mark_bar().encode(\n",
    "    x = alt.X('Month', sort=list(seasonal_data['Month'])),\n",
    "    y = 'Volume',\n",
    "    tooltip = ('Month', 'Volume'),\n",
    "    color = alt.value('#7CB9E8')\n",
    ").properties(\n",
    "    width = 800,\n",
    "    height = 300,\n",
    "    title = 'Crime Volume by Month'\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "fff1bbc1",
   "metadata": {},
   "source": [
    "### Heat Map of Date:Time Volume"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "5c22a351",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Month</th>\n",
       "      <th>Hour</th>\n",
       "      <th>Volume</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Jan</td>\n",
       "      <td>01</td>\n",
       "      <td>1030</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Jan</td>\n",
       "      <td>02</td>\n",
       "      <td>797</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Jan</td>\n",
       "      <td>03</td>\n",
       "      <td>605</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Jan</td>\n",
       "      <td>04</td>\n",
       "      <td>467</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Jan</td>\n",
       "      <td>05</td>\n",
       "      <td>427</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>283</th>\n",
       "      <td>Dec</td>\n",
       "      <td>20</td>\n",
       "      <td>2110</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>284</th>\n",
       "      <td>Dec</td>\n",
       "      <td>21</td>\n",
       "      <td>1845</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>285</th>\n",
       "      <td>Dec</td>\n",
       "      <td>22</td>\n",
       "      <td>1758</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>286</th>\n",
       "      <td>Dec</td>\n",
       "      <td>23</td>\n",
       "      <td>1537</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>287</th>\n",
       "      <td>Dec</td>\n",
       "      <td>24</td>\n",
       "      <td>1598</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>288 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    Month Hour  Volume\n",
       "0     Jan   01    1030\n",
       "1     Jan   02     797\n",
       "2     Jan   03     605\n",
       "3     Jan   04     467\n",
       "4     Jan   05     427\n",
       "..    ...  ...     ...\n",
       "283   Dec   20    2110\n",
       "284   Dec   21    1845\n",
       "285   Dec   22    1758\n",
       "286   Dec   23    1537\n",
       "287   Dec   24    1598\n",
       "\n",
       "[288 rows x 3 columns]"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "keep = ['Occur Date', 'Occur Time', 'Report Number']\n",
    "\n",
    "\n",
    "\n",
    "heat_data = df_union[keep] #  copy of data here to use in another model\n",
    "heat_data = heat_data.dropna()\n",
    "heat_data = heat_data.drop(heat_data[heat_data['Occur Date'] < '2009-01-01'].index).reset_index(drop = True)\n",
    "\n",
    "lst = []\n",
    "for m in heat_data['Occur Date']:\n",
    "    lst.append(m.month)\n",
    "heat_data['Occur Date'] = lst\n",
    "\n",
    "lst = []\n",
    "for t in heat_data['Occur Time']:\n",
    "    lst.append(t[0:2])\n",
    "heat_data['Occur Time'] = lst\n",
    "\n",
    "lst = []\n",
    "for n in heat_data['Report Number']:\n",
    "    lst.append(1)\n",
    "heat_data['Report Number'] = lst\n",
    "\n",
    "heat_data.rename(columns = {'Occur Date':'Month', 'Occur Time': 'Hour', 'Report Number':'Volume'}, inplace=True)\n",
    "\n",
    "# Fix Hour formatting\n",
    "lst = []\n",
    "for h in heat_data['Hour']:\n",
    "    if h[1] == ':': \n",
    "        lst.append('0'+h[0])\n",
    "    else:\n",
    "        lst.append(h)\n",
    "heat_data['Hour'] = lst\n",
    "lst = heat_data['Month'] \n",
    "sort = lst.unique()\n",
    "\n",
    "lst = []\n",
    "for h in heat_data['Hour']:\n",
    "    if h == '00':\n",
    "        lst.append('24')\n",
    "    else:\n",
    "        lst.append(h)\n",
    "heat_data['Hour'] = lst\n",
    "\n",
    "# Drop weird Hour\n",
    "heat_data = heat_data[heat_data['Hour']!='0T']\n",
    "\n",
    "\n",
    "# Group by month and hour\n",
    "heat_data = heat_data.groupby(['Month', 'Hour']).count().reset_index()\n",
    "\n",
    "# Converting into month name\n",
    "heat_data['Month'] = pd.to_datetime(heat_data['Month'], format='%m').dt.month_name().str.slice(stop=3)\n",
    "\n",
    "heat_data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "id": "6eda7ce0",
   "metadata": {},
   "outputs": [],
   "source": [
    "# heat map\n",
    "heat = alt.Chart(heat_data).mark_rect().encode(\n",
    "    x=alt.X('Hour'),\n",
    "    y=alt.Y('Month', sort= sort),\n",
    "    color=alt.Color('Volume'),\n",
    "    tooltip = ('Month', 'Hour', 'Volume')\n",
    ").properties(\n",
    "    width = 500,\n",
    "    height = 500,\n",
    "    title = 'Crime Volume by Hour/Month'\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "39f0121a",
   "metadata": {},
   "source": [
    "### Neighborhood Analysis"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "id": "ac3691fe",
   "metadata": {},
   "outputs": [],
   "source": [
    "keep = ['Neighborhood', 'Report Number']\n",
    "neighborhood_data = df_union[keep]\n",
    "neighborhood_data = neighborhood_data.dropna()\n",
    "neighborhood_data = neighborhood_data.drop_duplicates()\n",
    "neighborhood_data = neighborhood_data.groupby(by='Neighborhood').count()\n",
    "neighborhood_data = neighborhood_data.rename(columns ={'Report Number':'Volume'})\n",
    "neighborhood_data = neighborhood_data.sort_values(by='Volume', ascending=False).reset_index()\n",
    "neighborhood_data = neighborhood_data.head(20)\n",
    "\n",
    "alt.data_transformers.disable_max_rows()\n",
    "neighborhood = alt.Chart(neighborhood_data).mark_bar().encode(\n",
    "    x = 'Volume',\n",
    "    y = alt.Y('Neighborhood', sort = 'x')\n",
    ").properties(\n",
    "    width = 400,\n",
    "    height = 500,\n",
    "    title = 'Most Dangerous Neighborhoods'\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "cca04b0d",
   "metadata": {},
   "source": [
    "### Top Offenses "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "id": "6d30df32",
   "metadata": {},
   "outputs": [],
   "source": [
    "topo = df_union['UCR Literal']\n",
    "\n",
    "topo = topo.to_frame()\n",
    "topo['Volume'] = 1\n",
    "topo = topo.groupby(by = 'UCR Literal').sum()\n",
    "topo = topo.sort_values(by='Volume', ascending = False).reset_index()\n",
    "topo = topo.head(12)\n",
    "\n",
    "to = alt.Chart(topo).mark_bar().encode(\n",
    "    x = alt.X('UCR Literal',sort ='-y'),\n",
    "    y = 'Volume',\n",
    ").properties(\n",
    "    title = 'Top 10 Common Offenses',\n",
    "    width = 300,\n",
    "    height = 500\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "dfe7595b",
   "metadata": {},
   "source": [
    "# Visualizations\n",
    "Click on localhost link to view charts"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "id": "3b7ee33a",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "RendererRegistry.enable('mimetype')"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# alt.renderers.enable('default')\n",
    "# alt.renderers.enable('altair_viewer')\n",
    "alt.renderers.enable('mimetype')\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "id": "4958758c",
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "application/vnd.vegalite.v4+json": {
       "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json",
       "config": {
        "view": {
         "continuousHeight": 300,
         "continuousWidth": 400
        }
       },
       "datasets": {
        "data-1c7169a71a9a93ac65deef8a4a8406d9": [
         {
          "Month": "Jan",
          "Volume": 33244
         },
         {
          "Month": "Feb",
          "Volume": 26939
         },
         {
          "Month": "Mar",
          "Volume": 29780
         },
         {
          "Month": "Apr",
          "Volume": 31384
         },
         {
          "Month": "May",
          "Volume": 35001
         },
         {
          "Month": "Jun",
          "Volume": 35301
         },
         {
          "Month": "Jul",
          "Volume": 37203
         },
         {
          "Month": "Aug",
          "Volume": 36348
         },
         {
          "Month": "Sep",
          "Volume": 33019
         },
         {
          "Month": "Oct",
          "Volume": 34325
         },
         {
          "Month": "Nov",
          "Volume": 32812
         },
         {
          "Month": "Dec",
          "Volume": 33772
         }
        ],
        "data-f11de618d66d4277a24c982189e85d20": [
         {
          "90-day": null,
          "Occur Date": "2009-01-01T00:00:00",
          "Volume": 133
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-02T00:00:00",
          "Volume": 144
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-03T00:00:00",
          "Volume": 126
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-04T00:00:00",
          "Volume": 95
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-05T00:00:00",
          "Volume": 126
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-06T00:00:00",
          "Volume": 120
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-07T00:00:00",
          "Volume": 109
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-09T00:00:00",
          "Volume": 137
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-10T00:00:00",
          "Volume": 121
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-11T00:00:00",
          "Volume": 83
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-12T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-13T00:00:00",
          "Volume": 104
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-14T00:00:00",
          "Volume": 106
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-15T00:00:00",
          "Volume": 100
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-16T00:00:00",
          "Volume": 102
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-17T00:00:00",
          "Volume": 112
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-18T00:00:00",
          "Volume": 119
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-19T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-20T00:00:00",
          "Volume": 90
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-21T00:00:00",
          "Volume": 111
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-22T00:00:00",
          "Volume": 106
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-23T00:00:00",
          "Volume": 123
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-24T00:00:00",
          "Volume": 112
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-25T00:00:00",
          "Volume": 100
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-26T00:00:00",
          "Volume": 143
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-27T00:00:00",
          "Volume": 112
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-28T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-29T00:00:00",
          "Volume": 106
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-30T00:00:00",
          "Volume": 120
         },
         {
          "90-day": null,
          "Occur Date": "2009-01-31T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-01T00:00:00",
          "Volume": 113
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-02T00:00:00",
          "Volume": 97
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-03T00:00:00",
          "Volume": 101
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-04T00:00:00",
          "Volume": 91
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-05T00:00:00",
          "Volume": 97
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-06T00:00:00",
          "Volume": 101
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-07T00:00:00",
          "Volume": 91
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-08T00:00:00",
          "Volume": 93
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-09T00:00:00",
          "Volume": 109
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-10T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-11T00:00:00",
          "Volume": 100
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-12T00:00:00",
          "Volume": 91
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-13T00:00:00",
          "Volume": 114
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-14T00:00:00",
          "Volume": 73
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-15T00:00:00",
          "Volume": 79
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-16T00:00:00",
          "Volume": 71
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-17T00:00:00",
          "Volume": 91
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-18T00:00:00",
          "Volume": 105
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-19T00:00:00",
          "Volume": 96
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-20T00:00:00",
          "Volume": 111
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-22T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-23T00:00:00",
          "Volume": 77
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-24T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-25T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-26T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-27T00:00:00",
          "Volume": 101
         },
         {
          "90-day": null,
          "Occur Date": "2009-02-28T00:00:00",
          "Volume": 83
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-01T00:00:00",
          "Volume": 64
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-02T00:00:00",
          "Volume": 102
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-03T00:00:00",
          "Volume": 85
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-04T00:00:00",
          "Volume": 78
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-05T00:00:00",
          "Volume": 101
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-06T00:00:00",
          "Volume": 101
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-07T00:00:00",
          "Volume": 96
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-08T00:00:00",
          "Volume": 79
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-09T00:00:00",
          "Volume": 92
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-10T00:00:00",
          "Volume": 93
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-12T00:00:00",
          "Volume": 95
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-13T00:00:00",
          "Volume": 127
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-14T00:00:00",
          "Volume": 88
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-16T00:00:00",
          "Volume": 106
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-17T00:00:00",
          "Volume": 94
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-18T00:00:00",
          "Volume": 89
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-19T00:00:00",
          "Volume": 102
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-20T00:00:00",
          "Volume": 113
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-21T00:00:00",
          "Volume": 102
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-22T00:00:00",
          "Volume": 105
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-23T00:00:00",
          "Volume": 87
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-24T00:00:00",
          "Volume": 128
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-25T00:00:00",
          "Volume": 115
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-26T00:00:00",
          "Volume": 99
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-27T00:00:00",
          "Volume": 105
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-28T00:00:00",
          "Volume": 109
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-29T00:00:00",
          "Volume": 92
         },
         {
          "90-day": null,
          "Occur Date": "2009-03-30T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 100.97777777777777,
          "Occur Date": "2009-03-31T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 100.74444444444444,
          "Occur Date": "2009-04-01T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 100.42222222222222,
          "Occur Date": "2009-04-02T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 100.45555555555555,
          "Occur Date": "2009-04-03T00:00:00",
          "Volume": 129
         },
         {
          "90-day": 100.71111111111111,
          "Occur Date": "2009-04-04T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 100.16666666666667,
          "Occur Date": "2009-04-05T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 99.73333333333333,
          "Occur Date": "2009-04-06T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 99.6,
          "Occur Date": "2009-04-07T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.6,
          "Occur Date": "2009-04-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 99.08888888888889,
          "Occur Date": "2009-04-09T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 99.07777777777778,
          "Occur Date": "2009-04-10T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 99.1,
          "Occur Date": "2009-04-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 98.9,
          "Occur Date": "2009-04-12T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 98.66666666666667,
          "Occur Date": "2009-04-13T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 98.58888888888889,
          "Occur Date": "2009-04-14T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 98.54444444444445,
          "Occur Date": "2009-04-15T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 98.55555555555556,
          "Occur Date": "2009-04-16T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 98.53333333333333,
          "Occur Date": "2009-04-17T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 98.36666666666666,
          "Occur Date": "2009-04-18T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 98.4888888888889,
          "Occur Date": "2009-04-19T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 98.76666666666667,
          "Occur Date": "2009-04-20T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 98.66666666666667,
          "Occur Date": "2009-04-21T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 98.61111111111111,
          "Occur Date": "2009-04-22T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 98.36666666666666,
          "Occur Date": "2009-04-23T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 98.3,
          "Occur Date": "2009-04-24T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 98.52222222222223,
          "Occur Date": "2009-04-25T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 97.78888888888889,
          "Occur Date": "2009-04-26T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 97.5111111111111,
          "Occur Date": "2009-04-27T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 97.74444444444444,
          "Occur Date": "2009-04-28T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 97.92222222222222,
          "Occur Date": "2009-04-29T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 97.88888888888889,
          "Occur Date": "2009-04-30T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 98.36666666666666,
          "Occur Date": "2009-05-01T00:00:00",
          "Volume": 132
         },
         {
          "90-day": 98.38888888888889,
          "Occur Date": "2009-05-02T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 98.12222222222222,
          "Occur Date": "2009-05-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 98.2,
          "Occur Date": "2009-05-04T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 98.42222222222222,
          "Occur Date": "2009-05-05T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 98.62222222222222,
          "Occur Date": "2009-05-06T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 98.72222222222223,
          "Occur Date": "2009-05-07T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 99.03333333333333,
          "Occur Date": "2009-05-08T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 99.15555555555555,
          "Occur Date": "2009-05-09T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 99.4,
          "Occur Date": "2009-05-10T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 99.47777777777777,
          "Occur Date": "2009-05-11T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 99.46666666666667,
          "Occur Date": "2009-05-12T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 99.46666666666667,
          "Occur Date": "2009-05-13T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 99.42222222222222,
          "Occur Date": "2009-05-14T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 99.77777777777777,
          "Occur Date": "2009-05-15T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 99.97777777777777,
          "Occur Date": "2009-05-16T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 100.25555555555556,
          "Occur Date": "2009-05-17T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 100.38888888888889,
          "Occur Date": "2009-05-18T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 100.32222222222222,
          "Occur Date": "2009-05-19T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 100.58888888888889,
          "Occur Date": "2009-05-20T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 100.65555555555555,
          "Occur Date": "2009-05-21T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 101.33333333333333,
          "Occur Date": "2009-05-22T00:00:00",
          "Volume": 150
         },
         {
          "90-day": 101.64444444444445,
          "Occur Date": "2009-05-23T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 101.81111111111112,
          "Occur Date": "2009-05-24T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 102,
          "Occur Date": "2009-05-25T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 102.23333333333333,
          "Occur Date": "2009-05-26T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 102.36666666666666,
          "Occur Date": "2009-05-27T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 102.46666666666667,
          "Occur Date": "2009-05-28T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 102.95555555555555,
          "Occur Date": "2009-05-29T00:00:00",
          "Volume": 127
         },
         {
          "90-day": 103.63333333333334,
          "Occur Date": "2009-05-30T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 103.82222222222222,
          "Occur Date": "2009-05-31T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 104.34444444444445,
          "Occur Date": "2009-06-01T00:00:00",
          "Volume": 132
         },
         {
          "90-day": 105,
          "Occur Date": "2009-06-02T00:00:00",
          "Volume": 137
         },
         {
          "90-day": 105.27777777777777,
          "Occur Date": "2009-06-03T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 105.63333333333334,
          "Occur Date": "2009-06-04T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 106.06666666666666,
          "Occur Date": "2009-06-05T00:00:00",
          "Volume": 135
         },
         {
          "90-day": 106.52222222222223,
          "Occur Date": "2009-06-06T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 106.94444444444444,
          "Occur Date": "2009-06-07T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 107.45555555555555,
          "Occur Date": "2009-06-08T00:00:00",
          "Volume": 139
         },
         {
          "90-day": 107.83333333333333,
          "Occur Date": "2009-06-09T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 108.23333333333333,
          "Occur Date": "2009-06-10T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 108.18888888888888,
          "Occur Date": "2009-06-11T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 108.54444444444445,
          "Occur Date": "2009-06-12T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 108.8,
          "Occur Date": "2009-06-13T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 108.53333333333333,
          "Occur Date": "2009-06-14T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 108.64444444444445,
          "Occur Date": "2009-06-15T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 109.08888888888889,
          "Occur Date": "2009-06-16T00:00:00",
          "Volume": 129
         },
         {
          "90-day": 109.42222222222222,
          "Occur Date": "2009-06-17T00:00:00",
          "Volume": 132
         },
         {
          "90-day": 109.4888888888889,
          "Occur Date": "2009-06-18T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 109.7,
          "Occur Date": "2009-06-19T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 110.02222222222223,
          "Occur Date": "2009-06-20T00:00:00",
          "Volume": 134
         },
         {
          "90-day": 110.05555555555556,
          "Occur Date": "2009-06-21T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 109.81111111111112,
          "Occur Date": "2009-06-22T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 110.12222222222222,
          "Occur Date": "2009-06-23T00:00:00",
          "Volume": 143
         },
         {
          "90-day": 110.64444444444445,
          "Occur Date": "2009-06-24T00:00:00",
          "Volume": 146
         },
         {
          "90-day": 110.84444444444445,
          "Occur Date": "2009-06-25T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 111.16666666666667,
          "Occur Date": "2009-06-26T00:00:00",
          "Volume": 138
         },
         {
          "90-day": 111.4,
          "Occur Date": "2009-06-27T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 111.46666666666667,
          "Occur Date": "2009-06-28T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 111.72222222222223,
          "Occur Date": "2009-06-29T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 111.71111111111111,
          "Occur Date": "2009-06-30T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 111.91111111111111,
          "Occur Date": "2009-07-01T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 111.82222222222222,
          "Occur Date": "2009-07-02T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 111.87777777777778,
          "Occur Date": "2009-07-03T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 112.32222222222222,
          "Occur Date": "2009-07-04T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 112.41111111111111,
          "Occur Date": "2009-07-05T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 112.33333333333333,
          "Occur Date": "2009-07-06T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 112.4888888888889,
          "Occur Date": "2009-07-07T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 112.53333333333333,
          "Occur Date": "2009-07-08T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 112.65555555555555,
          "Occur Date": "2009-07-09T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 113.06666666666666,
          "Occur Date": "2009-07-10T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 113.47777777777777,
          "Occur Date": "2009-07-11T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 113.52222222222223,
          "Occur Date": "2009-07-12T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 113.72222222222223,
          "Occur Date": "2009-07-13T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 114.05555555555556,
          "Occur Date": "2009-07-14T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 114.38888888888889,
          "Occur Date": "2009-07-15T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 114.38888888888889,
          "Occur Date": "2009-07-16T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 114.4888888888889,
          "Occur Date": "2009-07-17T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 114.63333333333334,
          "Occur Date": "2009-07-18T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 114.5111111111111,
          "Occur Date": "2009-07-19T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 114.81111111111112,
          "Occur Date": "2009-07-20T00:00:00",
          "Volume": 129
         },
         {
          "90-day": 115.06666666666666,
          "Occur Date": "2009-07-21T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 115.11111111111111,
          "Occur Date": "2009-07-22T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 115.14444444444445,
          "Occur Date": "2009-07-23T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 115.41111111111111,
          "Occur Date": "2009-07-24T00:00:00",
          "Volume": 144
         },
         {
          "90-day": 115.95555555555555,
          "Occur Date": "2009-07-25T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 116.08888888888889,
          "Occur Date": "2009-07-26T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 116.28888888888889,
          "Occur Date": "2009-07-27T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 116.17777777777778,
          "Occur Date": "2009-07-28T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 116.14444444444445,
          "Occur Date": "2009-07-29T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 116.03333333333333,
          "Occur Date": "2009-07-30T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 116.08888888888889,
          "Occur Date": "2009-07-31T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 116.75555555555556,
          "Occur Date": "2009-08-01T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 116.91111111111111,
          "Occur Date": "2009-08-02T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 117.03333333333333,
          "Occur Date": "2009-08-03T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 117,
          "Occur Date": "2009-08-04T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 117.44444444444444,
          "Occur Date": "2009-08-05T00:00:00",
          "Volume": 150
         },
         {
          "90-day": 117.26666666666667,
          "Occur Date": "2009-08-06T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 117.33333333333333,
          "Occur Date": "2009-08-07T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 117,
          "Occur Date": "2009-08-08T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 117.27777777777777,
          "Occur Date": "2009-08-09T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 117.31111111111112,
          "Occur Date": "2009-08-10T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 117.42222222222222,
          "Occur Date": "2009-08-11T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 117.4,
          "Occur Date": "2009-08-12T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 117.34444444444445,
          "Occur Date": "2009-08-13T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 117.62222222222222,
          "Occur Date": "2009-08-14T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 117.77777777777777,
          "Occur Date": "2009-08-15T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 117.71111111111111,
          "Occur Date": "2009-08-16T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 117.56666666666666,
          "Occur Date": "2009-08-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 117.47777777777777,
          "Occur Date": "2009-08-18T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 117.53333333333333,
          "Occur Date": "2009-08-19T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 117.14444444444445,
          "Occur Date": "2009-08-20T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 117.13333333333334,
          "Occur Date": "2009-08-21T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 117.4,
          "Occur Date": "2009-08-22T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 117.33333333333333,
          "Occur Date": "2009-08-23T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 117.21111111111111,
          "Occur Date": "2009-08-24T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 117.06666666666666,
          "Occur Date": "2009-08-25T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 117.11111111111111,
          "Occur Date": "2009-08-26T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 116.93333333333334,
          "Occur Date": "2009-08-27T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 116.87777777777778,
          "Occur Date": "2009-08-28T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 117.0111111111111,
          "Occur Date": "2009-08-29T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 116.56666666666666,
          "Occur Date": "2009-08-30T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 116.27777777777777,
          "Occur Date": "2009-08-31T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 116.15555555555555,
          "Occur Date": "2009-09-01T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 115.66666666666667,
          "Occur Date": "2009-09-02T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 115.37777777777778,
          "Occur Date": "2009-09-03T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 115.47777777777777,
          "Occur Date": "2009-09-04T00:00:00",
          "Volume": 129
         },
         {
          "90-day": 115.41111111111111,
          "Occur Date": "2009-09-05T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 114.93333333333334,
          "Occur Date": "2009-09-06T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 114.77777777777777,
          "Occur Date": "2009-09-07T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 114.61111111111111,
          "Occur Date": "2009-09-08T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 114.28888888888889,
          "Occur Date": "2009-09-09T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 114.06666666666666,
          "Occur Date": "2009-09-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 113.93333333333334,
          "Occur Date": "2009-09-11T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 114.08888888888889,
          "Occur Date": "2009-09-12T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 113.83333333333333,
          "Occur Date": "2009-09-13T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 113.33333333333333,
          "Occur Date": "2009-09-14T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 113.06666666666666,
          "Occur Date": "2009-09-15T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 112.87777777777778,
          "Occur Date": "2009-09-16T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 112.6,
          "Occur Date": "2009-09-17T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 112.52222222222223,
          "Occur Date": "2009-09-18T00:00:00",
          "Volume": 127
         },
         {
          "90-day": 112.82222222222222,
          "Occur Date": "2009-09-19T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 112.46666666666667,
          "Occur Date": "2009-09-20T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 112,
          "Occur Date": "2009-09-21T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 111.33333333333333,
          "Occur Date": "2009-09-22T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 111.12222222222222,
          "Occur Date": "2009-09-23T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 110.76666666666667,
          "Occur Date": "2009-09-24T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 110.62222222222222,
          "Occur Date": "2009-09-25T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 110.5,
          "Occur Date": "2009-09-26T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 110.14444444444445,
          "Occur Date": "2009-09-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 110.17777777777778,
          "Occur Date": "2009-09-28T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 109.95555555555555,
          "Occur Date": "2009-09-29T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 109.82222222222222,
          "Occur Date": "2009-09-30T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 109.4888888888889,
          "Occur Date": "2009-10-01T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 109.28888888888889,
          "Occur Date": "2009-10-02T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 109.28888888888889,
          "Occur Date": "2009-10-03T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 109.17777777777778,
          "Occur Date": "2009-10-04T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 109,
          "Occur Date": "2009-10-05T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 109.14444444444445,
          "Occur Date": "2009-10-06T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 108.8,
          "Occur Date": "2009-10-07T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 108.52222222222223,
          "Occur Date": "2009-10-08T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 108.64444444444445,
          "Occur Date": "2009-10-09T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 108.87777777777778,
          "Occur Date": "2009-10-10T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 108.63333333333334,
          "Occur Date": "2009-10-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 108.41111111111111,
          "Occur Date": "2009-10-12T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 107.9888888888889,
          "Occur Date": "2009-10-13T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 107.9,
          "Occur Date": "2009-10-14T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 107.7,
          "Occur Date": "2009-10-15T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 107.65555555555555,
          "Occur Date": "2009-10-16T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 107.67777777777778,
          "Occur Date": "2009-10-17T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 107.13333333333334,
          "Occur Date": "2009-10-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 106.73333333333333,
          "Occur Date": "2009-10-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 106.8,
          "Occur Date": "2009-10-20T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 106.72222222222223,
          "Occur Date": "2009-10-21T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 106.14444444444445,
          "Occur Date": "2009-10-22T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 106.05555555555556,
          "Occur Date": "2009-10-23T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 106.22222222222223,
          "Occur Date": "2009-10-24T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 105.77777777777777,
          "Occur Date": "2009-10-25T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 106.03333333333333,
          "Occur Date": "2009-10-26T00:00:00",
          "Volume": 135
         },
         {
          "90-day": 105.85555555555555,
          "Occur Date": "2009-10-27T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 105.47777777777777,
          "Occur Date": "2009-10-28T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 105.43333333333334,
          "Occur Date": "2009-10-29T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 105.2,
          "Occur Date": "2009-10-30T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 105.34444444444445,
          "Occur Date": "2009-10-31T00:00:00",
          "Volume": 135
         },
         {
          "90-day": 105.24444444444444,
          "Occur Date": "2009-11-01T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 105.14444444444445,
          "Occur Date": "2009-11-02T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.78888888888889,
          "Occur Date": "2009-11-03T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 104.85555555555555,
          "Occur Date": "2009-11-04T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 104.88888888888889,
          "Occur Date": "2009-11-05T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 105.03333333333333,
          "Occur Date": "2009-11-06T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 104.88888888888889,
          "Occur Date": "2009-11-07T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 104.8,
          "Occur Date": "2009-11-08T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 104.97777777777777,
          "Occur Date": "2009-11-09T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 105.08888888888889,
          "Occur Date": "2009-11-10T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 105.21111111111111,
          "Occur Date": "2009-11-11T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 105.34444444444445,
          "Occur Date": "2009-11-12T00:00:00",
          "Volume": 134
         },
         {
          "90-day": 105.43333333333334,
          "Occur Date": "2009-11-13T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 105.45555555555555,
          "Occur Date": "2009-11-14T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 105.41111111111111,
          "Occur Date": "2009-11-15T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 105.44444444444444,
          "Occur Date": "2009-11-16T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 105.74444444444444,
          "Occur Date": "2009-11-17T00:00:00",
          "Volume": 149
         },
         {
          "90-day": 105.64444444444445,
          "Occur Date": "2009-11-18T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 105.77777777777777,
          "Occur Date": "2009-11-19T00:00:00",
          "Volume": 128
         },
         {
          "90-day": 105.96666666666667,
          "Occur Date": "2009-11-20T00:00:00",
          "Volume": 133
         },
         {
          "90-day": 106.14444444444445,
          "Occur Date": "2009-11-21T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 105.86666666666666,
          "Occur Date": "2009-11-22T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 105.95555555555555,
          "Occur Date": "2009-11-23T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 106,
          "Occur Date": "2009-11-24T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 105.88888888888889,
          "Occur Date": "2009-11-25T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 105.34444444444445,
          "Occur Date": "2009-11-26T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 105.22222222222223,
          "Occur Date": "2009-11-27T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 105.44444444444444,
          "Occur Date": "2009-11-28T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 105.13333333333334,
          "Occur Date": "2009-11-29T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 105.3,
          "Occur Date": "2009-11-30T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 105.68888888888888,
          "Occur Date": "2009-12-01T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 105.9,
          "Occur Date": "2009-12-02T00:00:00",
          "Volume": 128
         },
         {
          "90-day": 105.73333333333333,
          "Occur Date": "2009-12-03T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 106.02222222222223,
          "Occur Date": "2009-12-04T00:00:00",
          "Volume": 150
         },
         {
          "90-day": 106.54444444444445,
          "Occur Date": "2009-12-05T00:00:00",
          "Volume": 143
         },
         {
          "90-day": 106.54444444444445,
          "Occur Date": "2009-12-06T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 106.7,
          "Occur Date": "2009-12-07T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 106.95555555555555,
          "Occur Date": "2009-12-08T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 107.2,
          "Occur Date": "2009-12-09T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 107.25555555555556,
          "Occur Date": "2009-12-10T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 107.4,
          "Occur Date": "2009-12-11T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 107.61111111111111,
          "Occur Date": "2009-12-12T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 107.75555555555556,
          "Occur Date": "2009-12-13T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 107.84444444444445,
          "Occur Date": "2009-12-14T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 108.07777777777778,
          "Occur Date": "2009-12-15T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 108.18888888888888,
          "Occur Date": "2009-12-16T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 107.9888888888889,
          "Occur Date": "2009-12-17T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 107.9,
          "Occur Date": "2009-12-18T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 108.28888888888889,
          "Occur Date": "2009-12-19T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 108.14444444444445,
          "Occur Date": "2009-12-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 108.58888888888889,
          "Occur Date": "2009-12-21T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 108.72222222222223,
          "Occur Date": "2009-12-22T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 108.84444444444445,
          "Occur Date": "2009-12-23T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 108.71111111111111,
          "Occur Date": "2009-12-24T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 108.26666666666667,
          "Occur Date": "2009-12-25T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 108.06666666666666,
          "Occur Date": "2009-12-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 107.61111111111111,
          "Occur Date": "2009-12-27T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 107.5111111111111,
          "Occur Date": "2009-12-28T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 107.34444444444445,
          "Occur Date": "2009-12-29T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 107.73333333333333,
          "Occur Date": "2009-12-30T00:00:00",
          "Volume": 128
         },
         {
          "90-day": 107.83333333333333,
          "Occur Date": "2009-12-31T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 108.16666666666667,
          "Occur Date": "2010-01-01T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 108.38888888888889,
          "Occur Date": "2010-01-02T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 108.13333333333334,
          "Occur Date": "2010-01-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 108.08888888888889,
          "Occur Date": "2010-01-04T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 107.88888888888889,
          "Occur Date": "2010-01-05T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 107.94444444444444,
          "Occur Date": "2010-01-06T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 107.57777777777778,
          "Occur Date": "2010-01-07T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 107.15555555555555,
          "Occur Date": "2010-01-08T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 107.07777777777778,
          "Occur Date": "2010-01-09T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 106.63333333333334,
          "Occur Date": "2010-01-10T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 106.5111111111111,
          "Occur Date": "2010-01-11T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 106.5,
          "Occur Date": "2010-01-12T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 106.58888888888889,
          "Occur Date": "2010-01-13T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 106.28888888888889,
          "Occur Date": "2010-01-14T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 106.26666666666667,
          "Occur Date": "2010-01-15T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 106.35555555555555,
          "Occur Date": "2010-01-16T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 106.26666666666667,
          "Occur Date": "2010-01-17T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 105.9,
          "Occur Date": "2010-01-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 106.02222222222223,
          "Occur Date": "2010-01-19T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 105.97777777777777,
          "Occur Date": "2010-01-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 105.45555555555555,
          "Occur Date": "2010-01-21T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 105.28888888888889,
          "Occur Date": "2010-01-22T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 105.36666666666666,
          "Occur Date": "2010-01-23T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.6,
          "Occur Date": "2010-01-24T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 104.64444444444445,
          "Occur Date": "2010-01-25T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 104.5,
          "Occur Date": "2010-01-26T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 104.02222222222223,
          "Occur Date": "2010-01-27T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 103.66666666666667,
          "Occur Date": "2010-01-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 103.07777777777778,
          "Occur Date": "2010-01-29T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 102.91111111111111,
          "Occur Date": "2010-01-30T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 102.4888888888889,
          "Occur Date": "2010-01-31T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 102.05555555555556,
          "Occur Date": "2010-02-01T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 101.84444444444445,
          "Occur Date": "2010-02-02T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 101.5111111111111,
          "Occur Date": "2010-02-03T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 101.08888888888889,
          "Occur Date": "2010-02-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 100.81111111111112,
          "Occur Date": "2010-02-05T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 100.78888888888889,
          "Occur Date": "2010-02-06T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 100.1,
          "Occur Date": "2010-02-07T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 99.52222222222223,
          "Occur Date": "2010-02-08T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 99.06666666666666,
          "Occur Date": "2010-02-09T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 98.38888888888889,
          "Occur Date": "2010-02-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 97.71111111111111,
          "Occur Date": "2010-02-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 97.4,
          "Occur Date": "2010-02-12T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 97.02222222222223,
          "Occur Date": "2010-02-13T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 96.52222222222223,
          "Occur Date": "2010-02-14T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 95.53333333333333,
          "Occur Date": "2010-02-15T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 95.03333333333333,
          "Occur Date": "2010-02-16T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 94.46666666666667,
          "Occur Date": "2010-02-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 93.66666666666667,
          "Occur Date": "2010-02-18T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 93.15555555555555,
          "Occur Date": "2010-02-19T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 92.97777777777777,
          "Occur Date": "2010-02-20T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 92.47777777777777,
          "Occur Date": "2010-02-21T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 92.04444444444445,
          "Occur Date": "2010-02-22T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 91.77777777777777,
          "Occur Date": "2010-02-23T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 91.71111111111111,
          "Occur Date": "2010-02-24T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 91.22222222222223,
          "Occur Date": "2010-02-25T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 91.0111111111111,
          "Occur Date": "2010-02-26T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 91.04444444444445,
          "Occur Date": "2010-02-27T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 90.43333333333334,
          "Occur Date": "2010-02-28T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 90.27777777777777,
          "Occur Date": "2010-03-01T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 89.67777777777778,
          "Occur Date": "2010-03-02T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 89.34444444444445,
          "Occur Date": "2010-03-03T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2010-03-04T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 88.15555555555555,
          "Occur Date": "2010-03-05T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 88,
          "Occur Date": "2010-03-06T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 87.24444444444444,
          "Occur Date": "2010-03-07T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 86.72222222222223,
          "Occur Date": "2010-03-08T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 86.13333333333334,
          "Occur Date": "2010-03-09T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 85.76666666666667,
          "Occur Date": "2010-03-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 85.52222222222223,
          "Occur Date": "2010-03-11T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 85.65555555555555,
          "Occur Date": "2010-03-12T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 85.52222222222223,
          "Occur Date": "2010-03-13T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.03333333333333,
          "Occur Date": "2010-03-14T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 84.73333333333333,
          "Occur Date": "2010-03-15T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 84.34444444444445,
          "Occur Date": "2010-03-16T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 83.9888888888889,
          "Occur Date": "2010-03-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2010-03-18T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 83.65555555555555,
          "Occur Date": "2010-03-19T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 83.77777777777777,
          "Occur Date": "2010-03-20T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 83.05555555555556,
          "Occur Date": "2010-03-21T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 82.72222222222223,
          "Occur Date": "2010-03-22T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 82.32222222222222,
          "Occur Date": "2010-03-23T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 82.38888888888889,
          "Occur Date": "2010-03-24T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 82.72222222222223,
          "Occur Date": "2010-03-25T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 82.93333333333334,
          "Occur Date": "2010-03-26T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 83.1,
          "Occur Date": "2010-03-27T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 82.7,
          "Occur Date": "2010-03-28T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 82.71111111111111,
          "Occur Date": "2010-03-29T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 82.2,
          "Occur Date": "2010-03-30T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 81.88888888888889,
          "Occur Date": "2010-03-31T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.77777777777777,
          "Occur Date": "2010-04-01T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 81.81111111111112,
          "Occur Date": "2010-04-02T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 82.08888888888889,
          "Occur Date": "2010-04-03T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 81.78888888888889,
          "Occur Date": "2010-04-04T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 82.12222222222222,
          "Occur Date": "2010-04-05T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 82.02222222222223,
          "Occur Date": "2010-04-06T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 82.2,
          "Occur Date": "2010-04-07T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 82.44444444444444,
          "Occur Date": "2010-04-08T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 82.4888888888889,
          "Occur Date": "2010-04-09T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 83.16666666666667,
          "Occur Date": "2010-04-10T00:00:00",
          "Volume": 127
         },
         {
          "90-day": 83.25555555555556,
          "Occur Date": "2010-04-11T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 83.1,
          "Occur Date": "2010-04-12T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 83.06666666666666,
          "Occur Date": "2010-04-13T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 83.27777777777777,
          "Occur Date": "2010-04-14T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 83.26666666666667,
          "Occur Date": "2010-04-15T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 83.53333333333333,
          "Occur Date": "2010-04-16T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 83.88888888888889,
          "Occur Date": "2010-04-17T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 84.02222222222223,
          "Occur Date": "2010-04-18T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 83.76666666666667,
          "Occur Date": "2010-04-19T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 84.1,
          "Occur Date": "2010-04-20T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 84.5111111111111,
          "Occur Date": "2010-04-21T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 84.5,
          "Occur Date": "2010-04-22T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 84.58888888888889,
          "Occur Date": "2010-04-23T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 84.96666666666667,
          "Occur Date": "2010-04-24T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 84.9,
          "Occur Date": "2010-04-25T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 85.2,
          "Occur Date": "2010-04-26T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 85.56666666666666,
          "Occur Date": "2010-04-27T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2010-04-28T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 86.1,
          "Occur Date": "2010-04-29T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 86.05555555555556,
          "Occur Date": "2010-04-30T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 86.55555555555556,
          "Occur Date": "2010-05-01T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 86.53333333333333,
          "Occur Date": "2010-05-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.44444444444444,
          "Occur Date": "2010-05-03T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 86.5,
          "Occur Date": "2010-05-04T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 86.68888888888888,
          "Occur Date": "2010-05-05T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 86.83333333333333,
          "Occur Date": "2010-05-06T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 87.0111111111111,
          "Occur Date": "2010-05-07T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 87.42222222222222,
          "Occur Date": "2010-05-08T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 87.61111111111111,
          "Occur Date": "2010-05-09T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 87.82222222222222,
          "Occur Date": "2010-05-10T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.07777777777778,
          "Occur Date": "2010-05-11T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 88.65555555555555,
          "Occur Date": "2010-05-12T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 88.96666666666667,
          "Occur Date": "2010-05-13T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.68888888888888,
          "Occur Date": "2010-05-14T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 90.13333333333334,
          "Occur Date": "2010-05-15T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 90.63333333333334,
          "Occur Date": "2010-05-16T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 91.04444444444445,
          "Occur Date": "2010-05-17T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 91.35555555555555,
          "Occur Date": "2010-05-18T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 92.1,
          "Occur Date": "2010-05-19T00:00:00",
          "Volume": 128
         },
         {
          "90-day": 92.38888888888889,
          "Occur Date": "2010-05-20T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 92.8,
          "Occur Date": "2010-05-21T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 93.56666666666666,
          "Occur Date": "2010-05-22T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 93.74444444444444,
          "Occur Date": "2010-05-23T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 94.17777777777778,
          "Occur Date": "2010-05-24T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 94.56666666666666,
          "Occur Date": "2010-05-25T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.96666666666667,
          "Occur Date": "2010-05-26T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 95.12222222222222,
          "Occur Date": "2010-05-27T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 95.44444444444444,
          "Occur Date": "2010-05-28T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 95.76666666666667,
          "Occur Date": "2010-05-29T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 95.45555555555555,
          "Occur Date": "2010-05-30T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 95.74444444444444,
          "Occur Date": "2010-05-31T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 96.17777777777778,
          "Occur Date": "2010-06-01T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 96.31111111111112,
          "Occur Date": "2010-06-02T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 96.4888888888889,
          "Occur Date": "2010-06-03T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 96.67777777777778,
          "Occur Date": "2010-06-04T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 97.22222222222223,
          "Occur Date": "2010-06-05T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 97.31111111111112,
          "Occur Date": "2010-06-06T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 97.6,
          "Occur Date": "2010-06-07T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 97.93333333333334,
          "Occur Date": "2010-06-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 98.1,
          "Occur Date": "2010-06-09T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 98.02222222222223,
          "Occur Date": "2010-06-10T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 98.37777777777778,
          "Occur Date": "2010-06-11T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 98.66666666666667,
          "Occur Date": "2010-06-12T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 98.57777777777778,
          "Occur Date": "2010-06-13T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 98.87777777777778,
          "Occur Date": "2010-06-14T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 99.02222222222223,
          "Occur Date": "2010-06-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 99.03333333333333,
          "Occur Date": "2010-06-16T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 98.87777777777778,
          "Occur Date": "2010-06-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 99.06666666666666,
          "Occur Date": "2010-06-18T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 99.67777777777778,
          "Occur Date": "2010-06-19T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 99.55555555555556,
          "Occur Date": "2010-06-20T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 99.91111111111111,
          "Occur Date": "2010-06-21T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 100.1,
          "Occur Date": "2010-06-22T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 100.18888888888888,
          "Occur Date": "2010-06-23T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 100.33333333333333,
          "Occur Date": "2010-06-24T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 100.64444444444445,
          "Occur Date": "2010-06-25T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 101.13333333333334,
          "Occur Date": "2010-06-26T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 100.96666666666667,
          "Occur Date": "2010-06-27T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 101.23333333333333,
          "Occur Date": "2010-06-28T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 101.62222222222222,
          "Occur Date": "2010-06-29T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 101.7,
          "Occur Date": "2010-06-30T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 101.81111111111112,
          "Occur Date": "2010-07-01T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 101.9,
          "Occur Date": "2010-07-02T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 102.12222222222222,
          "Occur Date": "2010-07-03T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 101.77777777777777,
          "Occur Date": "2010-07-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 101.62222222222222,
          "Occur Date": "2010-07-05T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 101.56666666666666,
          "Occur Date": "2010-07-06T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 101.63333333333334,
          "Occur Date": "2010-07-07T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.7,
          "Occur Date": "2010-07-08T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.38888888888889,
          "Occur Date": "2010-07-09T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 101.47777777777777,
          "Occur Date": "2010-07-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 101.47777777777777,
          "Occur Date": "2010-07-11T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 101.62222222222222,
          "Occur Date": "2010-07-12T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 101.86666666666666,
          "Occur Date": "2010-07-13T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 102.0111111111111,
          "Occur Date": "2010-07-14T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 102.04444444444445,
          "Occur Date": "2010-07-15T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 102.04444444444445,
          "Occur Date": "2010-07-16T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 102.05555555555556,
          "Occur Date": "2010-07-17T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 102.07777777777778,
          "Occur Date": "2010-07-18T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 102.13333333333334,
          "Occur Date": "2010-07-19T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 102.16666666666667,
          "Occur Date": "2010-07-20T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 102.42222222222222,
          "Occur Date": "2010-07-21T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 102.44444444444444,
          "Occur Date": "2010-07-22T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 102.67777777777778,
          "Occur Date": "2010-07-23T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 102.78888888888889,
          "Occur Date": "2010-07-24T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 102.72222222222223,
          "Occur Date": "2010-07-25T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 102.9,
          "Occur Date": "2010-07-26T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 102.84444444444445,
          "Occur Date": "2010-07-27T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 103.02222222222223,
          "Occur Date": "2010-07-28T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 103.08888888888889,
          "Occur Date": "2010-07-29T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 103.26666666666667,
          "Occur Date": "2010-07-30T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 103.73333333333333,
          "Occur Date": "2010-07-31T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 103.85555555555555,
          "Occur Date": "2010-08-01T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 104.02222222222223,
          "Occur Date": "2010-08-02T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.12222222222222,
          "Occur Date": "2010-08-03T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 104.22222222222223,
          "Occur Date": "2010-08-04T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 103.96666666666667,
          "Occur Date": "2010-08-05T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 104.07777777777778,
          "Occur Date": "2010-08-06T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 104.46666666666667,
          "Occur Date": "2010-08-07T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 104.44444444444444,
          "Occur Date": "2010-08-08T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 104.41111111111111,
          "Occur Date": "2010-08-09T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 104.23333333333333,
          "Occur Date": "2010-08-10T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 104.24444444444444,
          "Occur Date": "2010-08-11T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.13333333333334,
          "Occur Date": "2010-08-12T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.08888888888889,
          "Occur Date": "2010-08-13T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 104.24444444444444,
          "Occur Date": "2010-08-14T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 104.27777777777777,
          "Occur Date": "2010-08-15T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 104.31111111111112,
          "Occur Date": "2010-08-16T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 103.94444444444444,
          "Occur Date": "2010-08-17T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 104.0111111111111,
          "Occur Date": "2010-08-18T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 103.96666666666667,
          "Occur Date": "2010-08-19T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 103.93333333333334,
          "Occur Date": "2010-08-20T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 104.11111111111111,
          "Occur Date": "2010-08-21T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 103.95555555555555,
          "Occur Date": "2010-08-22T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 104.22222222222223,
          "Occur Date": "2010-08-23T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 104.0111111111111,
          "Occur Date": "2010-08-24T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 103.91111111111111,
          "Occur Date": "2010-08-25T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 103.94444444444444,
          "Occur Date": "2010-08-26T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 104.15555555555555,
          "Occur Date": "2010-08-27T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 104.32222222222222,
          "Occur Date": "2010-08-28T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 104.05555555555556,
          "Occur Date": "2010-08-29T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 103.71111111111111,
          "Occur Date": "2010-08-30T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.8,
          "Occur Date": "2010-08-31T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 103.9,
          "Occur Date": "2010-09-01T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 103.66666666666667,
          "Occur Date": "2010-09-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 103.93333333333334,
          "Occur Date": "2010-09-03T00:00:00",
          "Volume": 135
         },
         {
          "90-day": 104.24444444444444,
          "Occur Date": "2010-09-04T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 104.41111111111111,
          "Occur Date": "2010-09-05T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 104.24444444444444,
          "Occur Date": "2010-09-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 104.42222222222222,
          "Occur Date": "2010-09-07T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 104.41111111111111,
          "Occur Date": "2010-09-08T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 104.25555555555556,
          "Occur Date": "2010-09-09T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.25555555555556,
          "Occur Date": "2010-09-10T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 104.32222222222222,
          "Occur Date": "2010-09-11T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 104.21111111111111,
          "Occur Date": "2010-09-12T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 104.27777777777777,
          "Occur Date": "2010-09-13T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 104.71111111111111,
          "Occur Date": "2010-09-14T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 104.85555555555555,
          "Occur Date": "2010-09-15T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.67777777777778,
          "Occur Date": "2010-09-16T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.61111111111111,
          "Occur Date": "2010-09-17T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 104.86666666666666,
          "Occur Date": "2010-09-18T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 104.56666666666666,
          "Occur Date": "2010-09-19T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 104.4888888888889,
          "Occur Date": "2010-09-20T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 104.5111111111111,
          "Occur Date": "2010-09-21T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 104.27777777777777,
          "Occur Date": "2010-09-22T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 104.1,
          "Occur Date": "2010-09-23T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.15555555555555,
          "Occur Date": "2010-09-24T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 104.28888888888889,
          "Occur Date": "2010-09-25T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.94444444444444,
          "Occur Date": "2010-09-26T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 103.8,
          "Occur Date": "2010-09-27T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 103.67777777777778,
          "Occur Date": "2010-09-28T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 103.53333333333333,
          "Occur Date": "2010-09-29T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 103.21111111111111,
          "Occur Date": "2010-09-30T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 103.5,
          "Occur Date": "2010-10-01T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 103.88888888888889,
          "Occur Date": "2010-10-02T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 103.83333333333333,
          "Occur Date": "2010-10-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 103.81111111111112,
          "Occur Date": "2010-10-04T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 103.87777777777778,
          "Occur Date": "2010-10-05T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 103.81111111111112,
          "Occur Date": "2010-10-06T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.9,
          "Occur Date": "2010-10-07T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 103.83333333333333,
          "Occur Date": "2010-10-08T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 103.78888888888889,
          "Occur Date": "2010-10-09T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 103.62222222222222,
          "Occur Date": "2010-10-10T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 103.6,
          "Occur Date": "2010-10-11T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 103.72222222222223,
          "Occur Date": "2010-10-12T00:00:00",
          "Volume": 127
         },
         {
          "90-day": 103.58888888888889,
          "Occur Date": "2010-10-13T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 103.52222222222223,
          "Occur Date": "2010-10-14T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 103.8,
          "Occur Date": "2010-10-15T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 104.0111111111111,
          "Occur Date": "2010-10-16T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 103.57777777777778,
          "Occur Date": "2010-10-17T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 103.6,
          "Occur Date": "2010-10-18T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 103.31111111111112,
          "Occur Date": "2010-10-19T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 103.41111111111111,
          "Occur Date": "2010-10-20T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 103.27777777777777,
          "Occur Date": "2010-10-21T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 103.55555555555556,
          "Occur Date": "2010-10-22T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 103.78888888888889,
          "Occur Date": "2010-10-23T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 103.56666666666666,
          "Occur Date": "2010-10-24T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 103.62222222222222,
          "Occur Date": "2010-10-25T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 103.71111111111111,
          "Occur Date": "2010-10-26T00:00:00",
          "Volume": 129
         },
         {
          "90-day": 103.91111111111111,
          "Occur Date": "2010-10-27T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 103.78888888888889,
          "Occur Date": "2010-10-28T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 103.75555555555556,
          "Occur Date": "2010-10-29T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 103.74444444444444,
          "Occur Date": "2010-10-30T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.63333333333334,
          "Occur Date": "2010-10-31T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 103.96666666666667,
          "Occur Date": "2010-11-01T00:00:00",
          "Volume": 132
         },
         {
          "90-day": 104.02222222222223,
          "Occur Date": "2010-11-02T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 104.22222222222223,
          "Occur Date": "2010-11-03T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.34444444444445,
          "Occur Date": "2010-11-04T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 104.22222222222223,
          "Occur Date": "2010-11-05T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 104.5,
          "Occur Date": "2010-11-06T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 104.23333333333333,
          "Occur Date": "2010-11-07T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 104.34444444444445,
          "Occur Date": "2010-11-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.31111111111112,
          "Occur Date": "2010-11-09T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 104.17777777777778,
          "Occur Date": "2010-11-10T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 104.35555555555555,
          "Occur Date": "2010-11-11T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 104.14444444444445,
          "Occur Date": "2010-11-12T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.27777777777777,
          "Occur Date": "2010-11-13T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 104.31111111111112,
          "Occur Date": "2010-11-14T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 104.33333333333333,
          "Occur Date": "2010-11-15T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 104.28888888888889,
          "Occur Date": "2010-11-16T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 104.37777777777778,
          "Occur Date": "2010-11-17T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 104.11111111111111,
          "Occur Date": "2010-11-18T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 104.23333333333333,
          "Occur Date": "2010-11-19T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 104.24444444444444,
          "Occur Date": "2010-11-20T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 103.64444444444445,
          "Occur Date": "2010-11-21T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 103.91111111111111,
          "Occur Date": "2010-11-22T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 104.26666666666667,
          "Occur Date": "2010-11-23T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 104.34444444444445,
          "Occur Date": "2010-11-24T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 103.78888888888889,
          "Occur Date": "2010-11-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 103.61111111111111,
          "Occur Date": "2010-11-26T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 103.83333333333333,
          "Occur Date": "2010-11-27T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 103.76666666666667,
          "Occur Date": "2010-11-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 103.85555555555555,
          "Occur Date": "2010-11-29T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 103.54444444444445,
          "Occur Date": "2010-11-30T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 104.02222222222223,
          "Occur Date": "2010-12-01T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 103.66666666666667,
          "Occur Date": "2010-12-02T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 103.87777777777778,
          "Occur Date": "2010-12-03T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 104.17777777777778,
          "Occur Date": "2010-12-04T00:00:00",
          "Volume": 137
         },
         {
          "90-day": 104.04444444444445,
          "Occur Date": "2010-12-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 103.87777777777778,
          "Occur Date": "2010-12-06T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 103.9,
          "Occur Date": "2010-12-07T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 103.77777777777777,
          "Occur Date": "2010-12-08T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.82222222222222,
          "Occur Date": "2010-12-09T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 103.8,
          "Occur Date": "2010-12-10T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.84444444444445,
          "Occur Date": "2010-12-11T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 103.54444444444445,
          "Occur Date": "2010-12-12T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 103.21111111111111,
          "Occur Date": "2010-12-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 103.17777777777778,
          "Occur Date": "2010-12-14T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 103.15555555555555,
          "Occur Date": "2010-12-15T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 102.96666666666667,
          "Occur Date": "2010-12-16T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 103.15555555555555,
          "Occur Date": "2010-12-17T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 103.17777777777778,
          "Occur Date": "2010-12-18T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 102.88888888888889,
          "Occur Date": "2010-12-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 102.97777777777777,
          "Occur Date": "2010-12-20T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 103.05555555555556,
          "Occur Date": "2010-12-21T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 103.0111111111111,
          "Occur Date": "2010-12-22T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 102.84444444444445,
          "Occur Date": "2010-12-23T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 102.78888888888889,
          "Occur Date": "2010-12-24T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 102.5,
          "Occur Date": "2010-12-25T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 102.06666666666666,
          "Occur Date": "2010-12-26T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 101.97777777777777,
          "Occur Date": "2010-12-27T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 101.81111111111112,
          "Occur Date": "2010-12-28T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 101.75555555555556,
          "Occur Date": "2010-12-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 101.52222222222223,
          "Occur Date": "2010-12-30T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 101.33333333333333,
          "Occur Date": "2010-12-31T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 101.78888888888889,
          "Occur Date": "2011-01-01T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 101.54444444444445,
          "Occur Date": "2011-01-02T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 101.54444444444445,
          "Occur Date": "2011-01-03T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 101.68888888888888,
          "Occur Date": "2011-01-04T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 101.77777777777777,
          "Occur Date": "2011-01-05T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 101.73333333333333,
          "Occur Date": "2011-01-06T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 101.93333333333334,
          "Occur Date": "2011-01-07T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 101.93333333333334,
          "Occur Date": "2011-01-08T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.4,
          "Occur Date": "2011-01-09T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 100.43333333333334,
          "Occur Date": "2011-01-10T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 99.75555555555556,
          "Occur Date": "2011-01-11T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 99.2,
          "Occur Date": "2011-01-12T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 98.84444444444445,
          "Occur Date": "2011-01-13T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 98.47777777777777,
          "Occur Date": "2011-01-14T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 98.37777777777778,
          "Occur Date": "2011-01-15T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 98.11111111111111,
          "Occur Date": "2011-01-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 97.92222222222222,
          "Occur Date": "2011-01-17T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 97.82222222222222,
          "Occur Date": "2011-01-18T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 97.58888888888889,
          "Occur Date": "2011-01-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 97.13333333333334,
          "Occur Date": "2011-01-20T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 96.93333333333334,
          "Occur Date": "2011-01-21T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 96.44444444444444,
          "Occur Date": "2011-01-22T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 96.06666666666666,
          "Occur Date": "2011-01-23T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 95.57777777777778,
          "Occur Date": "2011-01-24T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.18888888888888,
          "Occur Date": "2011-01-25T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 94.83333333333333,
          "Occur Date": "2011-01-26T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 94.35555555555555,
          "Occur Date": "2011-01-27T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 94.33333333333333,
          "Occur Date": "2011-01-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 93.95555555555555,
          "Occur Date": "2011-01-29T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 93.37777777777778,
          "Occur Date": "2011-01-30T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 92.88888888888889,
          "Occur Date": "2011-01-31T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 92.63333333333334,
          "Occur Date": "2011-02-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 92.22222222222223,
          "Occur Date": "2011-02-02T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 91.75555555555556,
          "Occur Date": "2011-02-03T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 91.18888888888888,
          "Occur Date": "2011-02-04T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 91.14444444444445,
          "Occur Date": "2011-02-05T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 90.43333333333334,
          "Occur Date": "2011-02-06T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 90.02222222222223,
          "Occur Date": "2011-02-07T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 89.77777777777777,
          "Occur Date": "2011-02-08T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 89.28888888888889,
          "Occur Date": "2011-02-09T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 89.0111111111111,
          "Occur Date": "2011-02-10T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 88.5111111111111,
          "Occur Date": "2011-02-11T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 88.04444444444445,
          "Occur Date": "2011-02-12T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 87.61111111111111,
          "Occur Date": "2011-02-13T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 87.33333333333333,
          "Occur Date": "2011-02-14T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 86.87777777777778,
          "Occur Date": "2011-02-15T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 86.47777777777777,
          "Occur Date": "2011-02-16T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 85.85555555555555,
          "Occur Date": "2011-02-17T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 85.62222222222222,
          "Occur Date": "2011-02-18T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 85.78888888888889,
          "Occur Date": "2011-02-19T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.38888888888889,
          "Occur Date": "2011-02-20T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 84.75555555555556,
          "Occur Date": "2011-02-21T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 84.42222222222222,
          "Occur Date": "2011-02-22T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 84.4,
          "Occur Date": "2011-02-23T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 84.45555555555555,
          "Occur Date": "2011-02-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 84.36666666666666,
          "Occur Date": "2011-02-25T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.26666666666667,
          "Occur Date": "2011-02-26T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.82222222222222,
          "Occur Date": "2011-02-27T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 83.78888888888889,
          "Occur Date": "2011-02-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 83.15555555555555,
          "Occur Date": "2011-03-01T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 82.84444444444445,
          "Occur Date": "2011-03-02T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 82.52222222222223,
          "Occur Date": "2011-03-03T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 81.9,
          "Occur Date": "2011-03-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 81.94444444444444,
          "Occur Date": "2011-03-05T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.5,
          "Occur Date": "2011-03-06T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 81.36666666666666,
          "Occur Date": "2011-03-07T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 81.17777777777778,
          "Occur Date": "2011-03-08T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 80.85555555555555,
          "Occur Date": "2011-03-09T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 80.86666666666666,
          "Occur Date": "2011-03-10T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 80.77777777777777,
          "Occur Date": "2011-03-11T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 80.91111111111111,
          "Occur Date": "2011-03-12T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 80.57777777777778,
          "Occur Date": "2011-03-13T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 80.28888888888889,
          "Occur Date": "2011-03-14T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 80.11111111111111,
          "Occur Date": "2011-03-15T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.91111111111111,
          "Occur Date": "2011-03-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 79.45555555555555,
          "Occur Date": "2011-03-17T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 79.43333333333334,
          "Occur Date": "2011-03-18T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.5,
          "Occur Date": "2011-03-19T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 79.08888888888889,
          "Occur Date": "2011-03-20T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 79.02222222222223,
          "Occur Date": "2011-03-21T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 78.87777777777778,
          "Occur Date": "2011-03-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 78.56666666666666,
          "Occur Date": "2011-03-23T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.5111111111111,
          "Occur Date": "2011-03-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.06666666666666,
          "Occur Date": "2011-03-25T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 79.36666666666666,
          "Occur Date": "2011-03-26T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 79.17777777777778,
          "Occur Date": "2011-03-27T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.23333333333333,
          "Occur Date": "2011-03-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 79.54444444444445,
          "Occur Date": "2011-03-29T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 79.68888888888888,
          "Occur Date": "2011-03-30T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 79.92222222222222,
          "Occur Date": "2011-03-31T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 79.93333333333334,
          "Occur Date": "2011-04-01T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 80.04444444444445,
          "Occur Date": "2011-04-02T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 80.0111111111111,
          "Occur Date": "2011-04-03T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 79.8,
          "Occur Date": "2011-04-04T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.62222222222222,
          "Occur Date": "2011-04-05T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 79.86666666666666,
          "Occur Date": "2011-04-06T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 80.0111111111111,
          "Occur Date": "2011-04-07T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 80.18888888888888,
          "Occur Date": "2011-04-08T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 80.46666666666667,
          "Occur Date": "2011-04-09T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 80.83333333333333,
          "Occur Date": "2011-04-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 81.64444444444445,
          "Occur Date": "2011-04-11T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 81.87777777777778,
          "Occur Date": "2011-04-12T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 82.17777777777778,
          "Occur Date": "2011-04-13T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 82.33333333333333,
          "Occur Date": "2011-04-14T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 82.87777777777778,
          "Occur Date": "2011-04-15T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 83,
          "Occur Date": "2011-04-16T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 83.05555555555556,
          "Occur Date": "2011-04-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 82.86666666666666,
          "Occur Date": "2011-04-18T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 83.12222222222222,
          "Occur Date": "2011-04-19T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2011-04-20T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2011-04-21T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 83.75555555555556,
          "Occur Date": "2011-04-22T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 84.0111111111111,
          "Occur Date": "2011-04-23T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 83.81111111111112,
          "Occur Date": "2011-04-24T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 84.15555555555555,
          "Occur Date": "2011-04-25T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 84.22222222222223,
          "Occur Date": "2011-04-26T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 84.54444444444445,
          "Occur Date": "2011-04-27T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 84.58888888888889,
          "Occur Date": "2011-04-28T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 85.13333333333334,
          "Occur Date": "2011-04-29T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 85.22222222222223,
          "Occur Date": "2011-04-30T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 85.74444444444444,
          "Occur Date": "2011-05-01T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 85.96666666666667,
          "Occur Date": "2011-05-02T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 86.35555555555555,
          "Occur Date": "2011-05-03T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 86.7,
          "Occur Date": "2011-05-04T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 87.02222222222223,
          "Occur Date": "2011-05-05T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 87.4888888888889,
          "Occur Date": "2011-05-06T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 88.37777777777778,
          "Occur Date": "2011-05-07T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 88.62222222222222,
          "Occur Date": "2011-05-08T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.02222222222223,
          "Occur Date": "2011-05-09T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 89.36666666666666,
          "Occur Date": "2011-05-10T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 89.92222222222222,
          "Occur Date": "2011-05-11T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 90.34444444444445,
          "Occur Date": "2011-05-12T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 90.73333333333333,
          "Occur Date": "2011-05-13T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 91.23333333333333,
          "Occur Date": "2011-05-14T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 91.14444444444445,
          "Occur Date": "2011-05-15T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 91.47777777777777,
          "Occur Date": "2011-05-16T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 91.84444444444445,
          "Occur Date": "2011-05-17T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 92.27777777777777,
          "Occur Date": "2011-05-18T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 92.27777777777777,
          "Occur Date": "2011-05-19T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 92.73333333333333,
          "Occur Date": "2011-05-20T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 93.21111111111111,
          "Occur Date": "2011-05-21T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 93.52222222222223,
          "Occur Date": "2011-05-22T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 93.5,
          "Occur Date": "2011-05-23T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 94.08888888888889,
          "Occur Date": "2011-05-24T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 94.24444444444444,
          "Occur Date": "2011-05-25T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.3,
          "Occur Date": "2011-05-26T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 94.82222222222222,
          "Occur Date": "2011-05-27T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 95.05555555555556,
          "Occur Date": "2011-05-28T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 94.97777777777777,
          "Occur Date": "2011-05-29T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 95.25555555555556,
          "Occur Date": "2011-05-30T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 95.52222222222223,
          "Occur Date": "2011-05-31T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 95.77777777777777,
          "Occur Date": "2011-06-01T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 95.81111111111112,
          "Occur Date": "2011-06-02T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 96.15555555555555,
          "Occur Date": "2011-06-03T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 96.32222222222222,
          "Occur Date": "2011-06-04T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 96.28888888888889,
          "Occur Date": "2011-06-05T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 96.57777777777778,
          "Occur Date": "2011-06-06T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 96.84444444444445,
          "Occur Date": "2011-06-07T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 96.93333333333334,
          "Occur Date": "2011-06-08T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 97.18888888888888,
          "Occur Date": "2011-06-09T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 97.55555555555556,
          "Occur Date": "2011-06-10T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 97.75555555555556,
          "Occur Date": "2011-06-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 97.9,
          "Occur Date": "2011-06-12T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 98.03333333333333,
          "Occur Date": "2011-06-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 98.5111111111111,
          "Occur Date": "2011-06-14T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 98.73333333333333,
          "Occur Date": "2011-06-15T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 98.97777777777777,
          "Occur Date": "2011-06-16T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 99.5,
          "Occur Date": "2011-06-17T00:00:00",
          "Volume": 131
         },
         {
          "90-day": 99.9888888888889,
          "Occur Date": "2011-06-18T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 100,
          "Occur Date": "2011-06-19T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 100.38888888888889,
          "Occur Date": "2011-06-20T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 100.47777777777777,
          "Occur Date": "2011-06-21T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 100.85555555555555,
          "Occur Date": "2011-06-22T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 101.0111111111111,
          "Occur Date": "2011-06-23T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 101.32222222222222,
          "Occur Date": "2011-06-24T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 101.58888888888889,
          "Occur Date": "2011-06-25T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 101.66666666666667,
          "Occur Date": "2011-06-26T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 101.58888888888889,
          "Occur Date": "2011-06-27T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.67777777777778,
          "Occur Date": "2011-06-28T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 101.52222222222223,
          "Occur Date": "2011-06-29T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 101.45555555555555,
          "Occur Date": "2011-06-30T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 101.97777777777777,
          "Occur Date": "2011-07-01T00:00:00",
          "Volume": 135
         },
         {
          "90-day": 101.94444444444444,
          "Occur Date": "2011-07-02T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 102.15555555555555,
          "Occur Date": "2011-07-03T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 102.11111111111111,
          "Occur Date": "2011-07-04T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 102.13333333333334,
          "Occur Date": "2011-07-05T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 101.95555555555555,
          "Occur Date": "2011-07-06T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.83333333333333,
          "Occur Date": "2011-07-07T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 102.0111111111111,
          "Occur Date": "2011-07-08T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 102.26666666666667,
          "Occur Date": "2011-07-09T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 101.94444444444444,
          "Occur Date": "2011-07-10T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 102.25555555555556,
          "Occur Date": "2011-07-11T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 102.32222222222222,
          "Occur Date": "2011-07-12T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 102.47777777777777,
          "Occur Date": "2011-07-13T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 102.2,
          "Occur Date": "2011-07-14T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 102.33333333333333,
          "Occur Date": "2011-07-15T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 102.65555555555555,
          "Occur Date": "2011-07-16T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 102.54444444444445,
          "Occur Date": "2011-07-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 102.66666666666667,
          "Occur Date": "2011-07-18T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 102.63333333333334,
          "Occur Date": "2011-07-19T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 102.82222222222222,
          "Occur Date": "2011-07-20T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 103.13333333333334,
          "Occur Date": "2011-07-21T00:00:00",
          "Volume": 137
         },
         {
          "90-day": 103.28888888888889,
          "Occur Date": "2011-07-22T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 103.85555555555555,
          "Occur Date": "2011-07-23T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 103.66666666666667,
          "Occur Date": "2011-07-24T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 104,
          "Occur Date": "2011-07-25T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 104.21111111111111,
          "Occur Date": "2011-07-26T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 104.52222222222223,
          "Occur Date": "2011-07-27T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 104.5111111111111,
          "Occur Date": "2011-07-28T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 104.64444444444445,
          "Occur Date": "2011-07-29T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 104.66666666666667,
          "Occur Date": "2011-07-30T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 104.78888888888889,
          "Occur Date": "2011-07-31T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 105.32222222222222,
          "Occur Date": "2011-08-01T00:00:00",
          "Volume": 159
         },
         {
          "90-day": 105.38888888888889,
          "Occur Date": "2011-08-02T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 105.56666666666666,
          "Occur Date": "2011-08-03T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 105.72222222222223,
          "Occur Date": "2011-08-04T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 105.63333333333334,
          "Occur Date": "2011-08-05T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 105.86666666666666,
          "Occur Date": "2011-08-06T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 105.71111111111111,
          "Occur Date": "2011-08-07T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 105.67777777777778,
          "Occur Date": "2011-08-08T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 105.31111111111112,
          "Occur Date": "2011-08-09T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 105.17777777777778,
          "Occur Date": "2011-08-10T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 105.07777777777778,
          "Occur Date": "2011-08-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 105.34444444444445,
          "Occur Date": "2011-08-12T00:00:00",
          "Volume": 127
         },
         {
          "90-day": 105.58888888888889,
          "Occur Date": "2011-08-13T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 105.44444444444444,
          "Occur Date": "2011-08-14T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 105.62222222222222,
          "Occur Date": "2011-08-15T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 105.4,
          "Occur Date": "2011-08-16T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 105.61111111111111,
          "Occur Date": "2011-08-17T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 105.5,
          "Occur Date": "2011-08-18T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 105.3,
          "Occur Date": "2011-08-19T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 105.44444444444444,
          "Occur Date": "2011-08-20T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 105.25555555555556,
          "Occur Date": "2011-08-21T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 105.23333333333333,
          "Occur Date": "2011-08-22T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 105.26666666666667,
          "Occur Date": "2011-08-23T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 105.7,
          "Occur Date": "2011-08-24T00:00:00",
          "Volume": 132
         },
         {
          "90-day": 105.35555555555555,
          "Occur Date": "2011-08-25T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 105.52222222222223,
          "Occur Date": "2011-08-26T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 105.96666666666667,
          "Occur Date": "2011-08-27T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 105.93333333333334,
          "Occur Date": "2011-08-28T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 106.05555555555556,
          "Occur Date": "2011-08-29T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 105.77777777777777,
          "Occur Date": "2011-08-30T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 105.97777777777777,
          "Occur Date": "2011-08-31T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 106.11111111111111,
          "Occur Date": "2011-09-01T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 106.32222222222222,
          "Occur Date": "2011-09-02T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 106.8,
          "Occur Date": "2011-09-03T00:00:00",
          "Volume": 134
         },
         {
          "90-day": 106.94444444444444,
          "Occur Date": "2011-09-04T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 106.71111111111111,
          "Occur Date": "2011-09-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 106.5,
          "Occur Date": "2011-09-06T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 106.44444444444444,
          "Occur Date": "2011-09-07T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 106.23333333333333,
          "Occur Date": "2011-09-08T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 106.46666666666667,
          "Occur Date": "2011-09-09T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 106.6,
          "Occur Date": "2011-09-10T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 106.42222222222222,
          "Occur Date": "2011-09-11T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 106.12222222222222,
          "Occur Date": "2011-09-12T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 105.96666666666667,
          "Occur Date": "2011-09-13T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 105.75555555555556,
          "Occur Date": "2011-09-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 105.5111111111111,
          "Occur Date": "2011-09-15T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 105.41111111111111,
          "Occur Date": "2011-09-16T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 105.61111111111111,
          "Occur Date": "2011-09-17T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 105.25555555555556,
          "Occur Date": "2011-09-18T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 105.42222222222222,
          "Occur Date": "2011-09-19T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 105.06666666666666,
          "Occur Date": "2011-09-20T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 104.83333333333333,
          "Occur Date": "2011-09-21T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 104.74444444444444,
          "Occur Date": "2011-09-22T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 104.82222222222222,
          "Occur Date": "2011-09-23T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 105.02222222222223,
          "Occur Date": "2011-09-24T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 104.96666666666667,
          "Occur Date": "2011-09-25T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 104.66666666666667,
          "Occur Date": "2011-09-26T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 104.7,
          "Occur Date": "2011-09-27T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 104.34444444444445,
          "Occur Date": "2011-09-28T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 103.8,
          "Occur Date": "2011-09-29T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 103.97777777777777,
          "Occur Date": "2011-09-30T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 104.03333333333333,
          "Occur Date": "2011-10-01T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 103.83333333333333,
          "Occur Date": "2011-10-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 103.66666666666667,
          "Occur Date": "2011-10-03T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 103.47777777777777,
          "Occur Date": "2011-10-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 103.37777777777778,
          "Occur Date": "2011-10-05T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 103.32222222222222,
          "Occur Date": "2011-10-06T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 103.34444444444445,
          "Occur Date": "2011-10-07T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 103.56666666666666,
          "Occur Date": "2011-10-08T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 103.61111111111111,
          "Occur Date": "2011-10-09T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 103.5111111111111,
          "Occur Date": "2011-10-10T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 103.33333333333333,
          "Occur Date": "2011-10-11T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 103.43333333333334,
          "Occur Date": "2011-10-12T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 103.16666666666667,
          "Occur Date": "2011-10-13T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 102.93333333333334,
          "Occur Date": "2011-10-14T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 103.33333333333333,
          "Occur Date": "2011-10-15T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 102.81111111111112,
          "Occur Date": "2011-10-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 102.82222222222222,
          "Occur Date": "2011-10-17T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 102.76666666666667,
          "Occur Date": "2011-10-18T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 102.41111111111111,
          "Occur Date": "2011-10-19T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 102.17777777777778,
          "Occur Date": "2011-10-20T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 101.95555555555555,
          "Occur Date": "2011-10-21T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.97777777777777,
          "Occur Date": "2011-10-22T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 101.72222222222223,
          "Occur Date": "2011-10-23T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 101.46666666666667,
          "Occur Date": "2011-10-24T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 101.07777777777778,
          "Occur Date": "2011-10-25T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 101.04444444444445,
          "Occur Date": "2011-10-26T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 100.97777777777777,
          "Occur Date": "2011-10-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 100.93333333333334,
          "Occur Date": "2011-10-28T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 100.85555555555555,
          "Occur Date": "2011-10-29T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 99.93333333333334,
          "Occur Date": "2011-10-30T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 100.15555555555555,
          "Occur Date": "2011-10-31T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 100.16666666666667,
          "Occur Date": "2011-11-01T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 99.81111111111112,
          "Occur Date": "2011-11-02T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 99.44444444444444,
          "Occur Date": "2011-11-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 99.63333333333334,
          "Occur Date": "2011-11-04T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 99.72222222222223,
          "Occur Date": "2011-11-05T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 99.4888888888889,
          "Occur Date": "2011-11-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 99.31111111111112,
          "Occur Date": "2011-11-07T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 99.34444444444445,
          "Occur Date": "2011-11-08T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.5,
          "Occur Date": "2011-11-09T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 99.25555555555556,
          "Occur Date": "2011-11-10T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 99.31111111111112,
          "Occur Date": "2011-11-11T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.56666666666666,
          "Occur Date": "2011-11-12T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 99.35555555555555,
          "Occur Date": "2011-11-13T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 99.41111111111111,
          "Occur Date": "2011-11-14T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 99.61111111111111,
          "Occur Date": "2011-11-15T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 99.47777777777777,
          "Occur Date": "2011-11-16T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 99.45555555555555,
          "Occur Date": "2011-11-17T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 99.21111111111111,
          "Occur Date": "2011-11-18T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 99.4888888888889,
          "Occur Date": "2011-11-19T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 99.02222222222223,
          "Occur Date": "2011-11-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 98.9,
          "Occur Date": "2011-11-21T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 98.42222222222222,
          "Occur Date": "2011-11-22T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 98.4888888888889,
          "Occur Date": "2011-11-23T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 97.97777777777777,
          "Occur Date": "2011-11-24T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 97.8,
          "Occur Date": "2011-11-25T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 98.04444444444445,
          "Occur Date": "2011-11-26T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 97.74444444444444,
          "Occur Date": "2011-11-27T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 97.85555555555555,
          "Occur Date": "2011-11-28T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 97.58888888888889,
          "Occur Date": "2011-11-29T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 97.22222222222223,
          "Occur Date": "2011-11-30T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 97.3,
          "Occur Date": "2011-12-01T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 96.82222222222222,
          "Occur Date": "2011-12-02T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 96.66666666666667,
          "Occur Date": "2011-12-03T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 96.7,
          "Occur Date": "2011-12-04T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 96.9,
          "Occur Date": "2011-12-05T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 96.83333333333333,
          "Occur Date": "2011-12-06T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 96.83333333333333,
          "Occur Date": "2011-12-07T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 96.81111111111112,
          "Occur Date": "2011-12-08T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 96.91111111111111,
          "Occur Date": "2011-12-09T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 97.05555555555556,
          "Occur Date": "2011-12-10T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 96.78888888888889,
          "Occur Date": "2011-12-11T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 96.93333333333334,
          "Occur Date": "2011-12-12T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 96.8,
          "Occur Date": "2011-12-13T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 96.92222222222222,
          "Occur Date": "2011-12-14T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 97.0111111111111,
          "Occur Date": "2011-12-15T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 97.15555555555555,
          "Occur Date": "2011-12-16T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 97.22222222222223,
          "Occur Date": "2011-12-17T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 96.93333333333334,
          "Occur Date": "2011-12-18T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 97.06666666666666,
          "Occur Date": "2011-12-19T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 97.27777777777777,
          "Occur Date": "2011-12-20T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 97.1,
          "Occur Date": "2011-12-21T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 96.88888888888889,
          "Occur Date": "2011-12-22T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 96.74444444444444,
          "Occur Date": "2011-12-23T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 96.76666666666667,
          "Occur Date": "2011-12-24T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 96.21111111111111,
          "Occur Date": "2011-12-25T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 95.88888888888889,
          "Occur Date": "2011-12-26T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 95.9888888888889,
          "Occur Date": "2011-12-27T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 96.03333333333333,
          "Occur Date": "2011-12-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.75555555555556,
          "Occur Date": "2011-12-29T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 95.54444444444445,
          "Occur Date": "2011-12-30T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 95.73333333333333,
          "Occur Date": "2011-12-31T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 95.71111111111111,
          "Occur Date": "2012-01-01T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 95.82222222222222,
          "Occur Date": "2012-01-02T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 95.68888888888888,
          "Occur Date": "2012-01-03T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 95.46666666666667,
          "Occur Date": "2012-01-04T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 95.32222222222222,
          "Occur Date": "2012-01-05T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.41111111111111,
          "Occur Date": "2012-01-06T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 95.26666666666667,
          "Occur Date": "2012-01-07T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 94.92222222222222,
          "Occur Date": "2012-01-08T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 94.92222222222222,
          "Occur Date": "2012-01-09T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 94.8,
          "Occur Date": "2012-01-10T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 94.87777777777778,
          "Occur Date": "2012-01-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 94.8,
          "Occur Date": "2012-01-12T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 94.57777777777778,
          "Occur Date": "2012-01-13T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 94.67777777777778,
          "Occur Date": "2012-01-14T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 94.4888888888889,
          "Occur Date": "2012-01-15T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 94.11111111111111,
          "Occur Date": "2012-01-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 93.96666666666667,
          "Occur Date": "2012-01-17T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 94.17777777777778,
          "Occur Date": "2012-01-18T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 93.81111111111112,
          "Occur Date": "2012-01-19T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 93.7,
          "Occur Date": "2012-01-20T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 93.62222222222222,
          "Occur Date": "2012-01-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 93.62222222222222,
          "Occur Date": "2012-01-22T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 93.52222222222223,
          "Occur Date": "2012-01-23T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 93.3,
          "Occur Date": "2012-01-24T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 93.06666666666666,
          "Occur Date": "2012-01-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 92.76666666666667,
          "Occur Date": "2012-01-26T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 92.65555555555555,
          "Occur Date": "2012-01-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 92.62222222222222,
          "Occur Date": "2012-01-28T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 91.83333333333333,
          "Occur Date": "2012-01-29T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 91.4888888888889,
          "Occur Date": "2012-01-30T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 91.44444444444444,
          "Occur Date": "2012-01-31T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 91.43333333333334,
          "Occur Date": "2012-02-01T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 91,
          "Occur Date": "2012-02-02T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 90.7,
          "Occur Date": "2012-02-03T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 90.38888888888889,
          "Occur Date": "2012-02-04T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 90.35555555555555,
          "Occur Date": "2012-02-05T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 90.16666666666667,
          "Occur Date": "2012-02-06T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 89.68888888888888,
          "Occur Date": "2012-02-07T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 89.02222222222223,
          "Occur Date": "2012-02-08T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 88.74444444444444,
          "Occur Date": "2012-02-09T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 88.47777777777777,
          "Occur Date": "2012-02-10T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 88.23333333333333,
          "Occur Date": "2012-02-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 87.95555555555555,
          "Occur Date": "2012-02-12T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 87.37777777777778,
          "Occur Date": "2012-02-13T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 86.92222222222222,
          "Occur Date": "2012-02-14T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 86.45555555555555,
          "Occur Date": "2012-02-15T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 86.16666666666667,
          "Occur Date": "2012-02-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 85.9888888888889,
          "Occur Date": "2012-02-17T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.9888888888889,
          "Occur Date": "2012-02-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 85.64444444444445,
          "Occur Date": "2012-02-19T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 85.63333333333334,
          "Occur Date": "2012-02-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 85.31111111111112,
          "Occur Date": "2012-02-21T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 85.53333333333333,
          "Occur Date": "2012-02-22T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 85.35555555555555,
          "Occur Date": "2012-02-23T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 84.97777777777777,
          "Occur Date": "2012-02-24T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 84.93333333333334,
          "Occur Date": "2012-02-25T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 84.36666666666666,
          "Occur Date": "2012-02-26T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 84.38888888888889,
          "Occur Date": "2012-02-27T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 84.0111111111111,
          "Occur Date": "2012-02-28T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 83.61111111111111,
          "Occur Date": "2012-02-29T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 83.37777777777778,
          "Occur Date": "2012-03-01T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 83.06666666666666,
          "Occur Date": "2012-03-02T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 83,
          "Occur Date": "2012-03-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 82.68888888888888,
          "Occur Date": "2012-03-04T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 82.36666666666666,
          "Occur Date": "2012-03-05T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 82.23333333333333,
          "Occur Date": "2012-03-06T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.86666666666666,
          "Occur Date": "2012-03-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 81.28888888888889,
          "Occur Date": "2012-03-08T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 81.23333333333333,
          "Occur Date": "2012-03-09T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 81.45555555555555,
          "Occur Date": "2012-03-10T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 81.1,
          "Occur Date": "2012-03-11T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 81.16666666666667,
          "Occur Date": "2012-03-12T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 80.58888888888889,
          "Occur Date": "2012-03-13T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 80.34444444444445,
          "Occur Date": "2012-03-14T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 79.76666666666667,
          "Occur Date": "2012-03-15T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 79.75555555555556,
          "Occur Date": "2012-03-16T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 80.26666666666667,
          "Occur Date": "2012-03-17T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 79.94444444444444,
          "Occur Date": "2012-03-18T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 79.6,
          "Occur Date": "2012-03-19T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.5111111111111,
          "Occur Date": "2012-03-20T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.74444444444444,
          "Occur Date": "2012-03-21T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 79.36666666666666,
          "Occur Date": "2012-03-22T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 79.53333333333333,
          "Occur Date": "2012-03-23T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 79.83333333333333,
          "Occur Date": "2012-03-24T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 79.83333333333333,
          "Occur Date": "2012-03-25T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 80.24444444444444,
          "Occur Date": "2012-03-26T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 80.07777777777778,
          "Occur Date": "2012-03-27T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 80.1,
          "Occur Date": "2012-03-28T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 79.94444444444444,
          "Occur Date": "2012-03-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.91111111111111,
          "Occur Date": "2012-03-30T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2012-03-31T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 79.7,
          "Occur Date": "2012-04-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.76666666666667,
          "Occur Date": "2012-04-02T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 79.91111111111111,
          "Occur Date": "2012-04-03T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 80.04444444444445,
          "Occur Date": "2012-04-04T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 79.81111111111112,
          "Occur Date": "2012-04-05T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 79.67777777777778,
          "Occur Date": "2012-04-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 79.87777777777778,
          "Occur Date": "2012-04-07T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 79.4,
          "Occur Date": "2012-04-08T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 79.11111111111111,
          "Occur Date": "2012-04-09T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 78.87777777777778,
          "Occur Date": "2012-04-10T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.78888888888889,
          "Occur Date": "2012-04-11T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 78.71111111111111,
          "Occur Date": "2012-04-12T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 78.91111111111111,
          "Occur Date": "2012-04-13T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 79.27777777777777,
          "Occur Date": "2012-04-14T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 79.3,
          "Occur Date": "2012-04-15T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.03333333333333,
          "Occur Date": "2012-04-16T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 78.9,
          "Occur Date": "2012-04-17T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 79.04444444444445,
          "Occur Date": "2012-04-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 78.95555555555555,
          "Occur Date": "2012-04-19T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.24444444444444,
          "Occur Date": "2012-04-20T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 79.31111111111112,
          "Occur Date": "2012-04-21T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 79.27777777777777,
          "Occur Date": "2012-04-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 79.5111111111111,
          "Occur Date": "2012-04-23T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 79.62222222222222,
          "Occur Date": "2012-04-24T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 79.77777777777777,
          "Occur Date": "2012-04-25T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 79.57777777777778,
          "Occur Date": "2012-04-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.8,
          "Occur Date": "2012-04-27T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 80.22222222222223,
          "Occur Date": "2012-04-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 80.33333333333333,
          "Occur Date": "2012-04-29T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 80.4,
          "Occur Date": "2012-04-30T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 80.47777777777777,
          "Occur Date": "2012-05-01T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 80.54444444444445,
          "Occur Date": "2012-05-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 80.72222222222223,
          "Occur Date": "2012-05-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 81.2,
          "Occur Date": "2012-05-04T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 81.52222222222223,
          "Occur Date": "2012-05-05T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 81.61111111111111,
          "Occur Date": "2012-05-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 81.84444444444445,
          "Occur Date": "2012-05-07T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 82.46666666666667,
          "Occur Date": "2012-05-08T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 82.81111111111112,
          "Occur Date": "2012-05-09T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 82.87777777777778,
          "Occur Date": "2012-05-10T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 83.22222222222223,
          "Occur Date": "2012-05-11T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 83.72222222222223,
          "Occur Date": "2012-05-12T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2012-05-13T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 84.11111111111111,
          "Occur Date": "2012-05-14T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 84.62222222222222,
          "Occur Date": "2012-05-15T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 84.93333333333334,
          "Occur Date": "2012-05-16T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 84.9,
          "Occur Date": "2012-05-17T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 85.25555555555556,
          "Occur Date": "2012-05-18T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 85.65555555555555,
          "Occur Date": "2012-05-19T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.41111111111111,
          "Occur Date": "2012-05-20T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 85.75555555555556,
          "Occur Date": "2012-05-21T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 85.96666666666667,
          "Occur Date": "2012-05-22T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 86.06666666666666,
          "Occur Date": "2012-05-23T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 86.35555555555555,
          "Occur Date": "2012-05-24T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 86.78888888888889,
          "Occur Date": "2012-05-25T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 87.3,
          "Occur Date": "2012-05-26T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 87.47777777777777,
          "Occur Date": "2012-05-27T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 87.84444444444445,
          "Occur Date": "2012-05-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.26666666666667,
          "Occur Date": "2012-05-29T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 88.74444444444444,
          "Occur Date": "2012-05-30T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 88.92222222222222,
          "Occur Date": "2012-05-31T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 89.3,
          "Occur Date": "2012-06-01T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 89.72222222222223,
          "Occur Date": "2012-06-02T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 90.07777777777778,
          "Occur Date": "2012-06-03T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 90.08888888888889,
          "Occur Date": "2012-06-04T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 90.61111111111111,
          "Occur Date": "2012-06-05T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 91.02222222222223,
          "Occur Date": "2012-06-06T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 91.35555555555555,
          "Occur Date": "2012-06-07T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 91.53333333333333,
          "Occur Date": "2012-06-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 92.1,
          "Occur Date": "2012-06-09T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 92.16666666666667,
          "Occur Date": "2012-06-10T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 92.66666666666667,
          "Occur Date": "2012-06-11T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 92.84444444444445,
          "Occur Date": "2012-06-12T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 93.04444444444445,
          "Occur Date": "2012-06-13T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 93.25555555555556,
          "Occur Date": "2012-06-14T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 93.18888888888888,
          "Occur Date": "2012-06-15T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 93.7,
          "Occur Date": "2012-06-16T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 93.81111111111112,
          "Occur Date": "2012-06-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 93.92222222222222,
          "Occur Date": "2012-06-18T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 93.76666666666667,
          "Occur Date": "2012-06-19T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 94.17777777777778,
          "Occur Date": "2012-06-20T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 93.92222222222222,
          "Occur Date": "2012-06-21T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 94.44444444444444,
          "Occur Date": "2012-06-22T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 94.71111111111111,
          "Occur Date": "2012-06-23T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 94.31111111111112,
          "Occur Date": "2012-06-24T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 94.74444444444444,
          "Occur Date": "2012-06-25T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 94.77777777777777,
          "Occur Date": "2012-06-26T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 94.95555555555555,
          "Occur Date": "2012-06-27T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 95.0111111111111,
          "Occur Date": "2012-06-28T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 95.32222222222222,
          "Occur Date": "2012-06-29T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 95.26666666666667,
          "Occur Date": "2012-06-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 95.46666666666667,
          "Occur Date": "2012-07-01T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 95.56666666666666,
          "Occur Date": "2012-07-02T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 95.46666666666667,
          "Occur Date": "2012-07-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 95.31111111111112,
          "Occur Date": "2012-07-04T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 95.34444444444445,
          "Occur Date": "2012-07-05T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 95.12222222222222,
          "Occur Date": "2012-07-06T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 95.62222222222222,
          "Occur Date": "2012-07-07T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 95.81111111111112,
          "Occur Date": "2012-07-08T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 96.1,
          "Occur Date": "2012-07-09T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 96.53333333333333,
          "Occur Date": "2012-07-10T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 96.5,
          "Occur Date": "2012-07-11T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 96.53333333333333,
          "Occur Date": "2012-07-12T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 96.43333333333334,
          "Occur Date": "2012-07-13T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 96.5,
          "Occur Date": "2012-07-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 96.75555555555556,
          "Occur Date": "2012-07-15T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 96.7,
          "Occur Date": "2012-07-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 97.07777777777778,
          "Occur Date": "2012-07-17T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 97.24444444444444,
          "Occur Date": "2012-07-18T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 97.12222222222222,
          "Occur Date": "2012-07-19T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 97.25555555555556,
          "Occur Date": "2012-07-20T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 97.55555555555556,
          "Occur Date": "2012-07-21T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 97.47777777777777,
          "Occur Date": "2012-07-22T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 97.8,
          "Occur Date": "2012-07-23T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 97.75555555555556,
          "Occur Date": "2012-07-24T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 98.13333333333334,
          "Occur Date": "2012-07-25T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 98.34444444444445,
          "Occur Date": "2012-07-26T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 98.55555555555556,
          "Occur Date": "2012-07-27T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 98.86666666666666,
          "Occur Date": "2012-07-28T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 98.93333333333334,
          "Occur Date": "2012-07-29T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.18888888888888,
          "Occur Date": "2012-07-30T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 99.33333333333333,
          "Occur Date": "2012-07-31T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 99.8,
          "Occur Date": "2012-08-01T00:00:00",
          "Volume": 130
         },
         {
          "90-day": 99.76666666666667,
          "Occur Date": "2012-08-02T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.88888888888889,
          "Occur Date": "2012-08-03T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 100.21111111111111,
          "Occur Date": "2012-08-04T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 100.14444444444445,
          "Occur Date": "2012-08-05T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 100.15555555555555,
          "Occur Date": "2012-08-06T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 100.06666666666666,
          "Occur Date": "2012-08-07T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 100.12222222222222,
          "Occur Date": "2012-08-08T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 100.08888888888889,
          "Occur Date": "2012-08-09T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 100.13333333333334,
          "Occur Date": "2012-08-10T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 100.31111111111112,
          "Occur Date": "2012-08-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 100.08888888888889,
          "Occur Date": "2012-08-12T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 99.85555555555555,
          "Occur Date": "2012-08-13T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 99.75555555555556,
          "Occur Date": "2012-08-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 99.96666666666667,
          "Occur Date": "2012-08-15T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 99.77777777777777,
          "Occur Date": "2012-08-16T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 99.95555555555555,
          "Occur Date": "2012-08-17T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 100.38888888888889,
          "Occur Date": "2012-08-18T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 100.31111111111112,
          "Occur Date": "2012-08-19T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 100.28888888888889,
          "Occur Date": "2012-08-20T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 100.28888888888889,
          "Occur Date": "2012-08-21T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 100.1,
          "Occur Date": "2012-08-22T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 99.87777777777778,
          "Occur Date": "2012-08-23T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 99.91111111111111,
          "Occur Date": "2012-08-24T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 100,
          "Occur Date": "2012-08-25T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 100,
          "Occur Date": "2012-08-26T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 99.91111111111111,
          "Occur Date": "2012-08-27T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 99.5,
          "Occur Date": "2012-08-28T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 99.57777777777778,
          "Occur Date": "2012-08-29T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 99.64444444444445,
          "Occur Date": "2012-08-30T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 99.8,
          "Occur Date": "2012-08-31T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 99.93333333333334,
          "Occur Date": "2012-09-01T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 100.08888888888889,
          "Occur Date": "2012-09-02T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 99.76666666666667,
          "Occur Date": "2012-09-03T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 99.83333333333333,
          "Occur Date": "2012-09-04T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 99.4888888888889,
          "Occur Date": "2012-09-05T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 99.32222222222222,
          "Occur Date": "2012-09-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 99.45555555555555,
          "Occur Date": "2012-09-07T00:00:00",
          "Volume": 124
         },
         {
          "90-day": 99.67777777777778,
          "Occur Date": "2012-09-08T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 99.4,
          "Occur Date": "2012-09-09T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 99.33333333333333,
          "Occur Date": "2012-09-10T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 99.21111111111111,
          "Occur Date": "2012-09-11T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 99.37777777777778,
          "Occur Date": "2012-09-12T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 99.18888888888888,
          "Occur Date": "2012-09-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 99.04444444444445,
          "Occur Date": "2012-09-14T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 99.22222222222223,
          "Occur Date": "2012-09-15T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 99.14444444444445,
          "Occur Date": "2012-09-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 98.96666666666667,
          "Occur Date": "2012-09-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 98.77777777777777,
          "Occur Date": "2012-09-18T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 98.7,
          "Occur Date": "2012-09-19T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 98.43333333333334,
          "Occur Date": "2012-09-20T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 98.38888888888889,
          "Occur Date": "2012-09-21T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 98.41111111111111,
          "Occur Date": "2012-09-22T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 97.96666666666667,
          "Occur Date": "2012-09-23T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 97.83333333333333,
          "Occur Date": "2012-09-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 97.84444444444445,
          "Occur Date": "2012-09-25T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 97.74444444444444,
          "Occur Date": "2012-09-26T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 97.44444444444444,
          "Occur Date": "2012-09-27T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 97.77777777777777,
          "Occur Date": "2012-09-28T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 97.8,
          "Occur Date": "2012-09-29T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 97.4,
          "Occur Date": "2012-09-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 97.42222222222222,
          "Occur Date": "2012-10-01T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 97.37777777777778,
          "Occur Date": "2012-10-02T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 97.38888888888889,
          "Occur Date": "2012-10-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 97.4888888888889,
          "Occur Date": "2012-10-04T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 97.85555555555555,
          "Occur Date": "2012-10-05T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 98.06666666666666,
          "Occur Date": "2012-10-06T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 98.04444444444445,
          "Occur Date": "2012-10-07T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 97.62222222222222,
          "Occur Date": "2012-10-08T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 97.57777777777778,
          "Occur Date": "2012-10-09T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 97.35555555555555,
          "Occur Date": "2012-10-10T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 97.25555555555556,
          "Occur Date": "2012-10-11T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 97.61111111111111,
          "Occur Date": "2012-10-12T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 97.91111111111111,
          "Occur Date": "2012-10-13T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 97.94444444444444,
          "Occur Date": "2012-10-14T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 97.94444444444444,
          "Occur Date": "2012-10-15T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 97.8,
          "Occur Date": "2012-10-16T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 97.56666666666666,
          "Occur Date": "2012-10-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 97.26666666666667,
          "Occur Date": "2012-10-18T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 97.16666666666667,
          "Occur Date": "2012-10-19T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 97,
          "Occur Date": "2012-10-20T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 96.54444444444445,
          "Occur Date": "2012-10-21T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 96.4,
          "Occur Date": "2012-10-22T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 96.26666666666667,
          "Occur Date": "2012-10-23T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 95.97777777777777,
          "Occur Date": "2012-10-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 95.97777777777777,
          "Occur Date": "2012-10-25T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 95.84444444444445,
          "Occur Date": "2012-10-26T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 95.76666666666667,
          "Occur Date": "2012-10-27T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.5111111111111,
          "Occur Date": "2012-10-28T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 95.43333333333334,
          "Occur Date": "2012-10-29T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 95.05555555555556,
          "Occur Date": "2012-10-30T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 95.11111111111111,
          "Occur Date": "2012-10-31T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 94.96666666666667,
          "Occur Date": "2012-11-01T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.75555555555556,
          "Occur Date": "2012-11-02T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 94.97777777777777,
          "Occur Date": "2012-11-03T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 94.85555555555555,
          "Occur Date": "2012-11-04T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 94.84444444444445,
          "Occur Date": "2012-11-05T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 94.95555555555555,
          "Occur Date": "2012-11-06T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.83333333333333,
          "Occur Date": "2012-11-07T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 94.57777777777778,
          "Occur Date": "2012-11-08T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 94.66666666666667,
          "Occur Date": "2012-11-09T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 94.74444444444444,
          "Occur Date": "2012-11-10T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 94.68888888888888,
          "Occur Date": "2012-11-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 94.76666666666667,
          "Occur Date": "2012-11-12T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 94.71111111111111,
          "Occur Date": "2012-11-13T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 94.76666666666667,
          "Occur Date": "2012-11-14T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.52222222222223,
          "Occur Date": "2012-11-15T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 94.52222222222223,
          "Occur Date": "2012-11-16T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 94.43333333333334,
          "Occur Date": "2012-11-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 94.23333333333333,
          "Occur Date": "2012-11-18T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 94.26666666666667,
          "Occur Date": "2012-11-19T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 94.27777777777777,
          "Occur Date": "2012-11-20T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 94.05555555555556,
          "Occur Date": "2012-11-21T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 93.52222222222223,
          "Occur Date": "2012-11-22T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 93.25555555555556,
          "Occur Date": "2012-11-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 93.27777777777777,
          "Occur Date": "2012-11-24T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 93.12222222222222,
          "Occur Date": "2012-11-25T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 93.26666666666667,
          "Occur Date": "2012-11-26T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 93.0111111111111,
          "Occur Date": "2012-11-27T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 92.83333333333333,
          "Occur Date": "2012-11-28T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 92.32222222222222,
          "Occur Date": "2012-11-29T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 92.07777777777778,
          "Occur Date": "2012-11-30T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 92.17777777777778,
          "Occur Date": "2012-12-01T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 92.22222222222223,
          "Occur Date": "2012-12-02T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 92.03333333333333,
          "Occur Date": "2012-12-03T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 92.0111111111111,
          "Occur Date": "2012-12-04T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 92.06666666666666,
          "Occur Date": "2012-12-05T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 91.7,
          "Occur Date": "2012-12-06T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 91.54444444444445,
          "Occur Date": "2012-12-07T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 91.58888888888889,
          "Occur Date": "2012-12-08T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 91.27777777777777,
          "Occur Date": "2012-12-09T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 91.57777777777778,
          "Occur Date": "2012-12-10T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 91.07777777777778,
          "Occur Date": "2012-12-11T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 91.0111111111111,
          "Occur Date": "2012-12-12T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.04444444444445,
          "Occur Date": "2012-12-13T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 90.9888888888889,
          "Occur Date": "2012-12-14T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 91.21111111111111,
          "Occur Date": "2012-12-15T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 91,
          "Occur Date": "2012-12-16T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 91.24444444444444,
          "Occur Date": "2012-12-17T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 91.6,
          "Occur Date": "2012-12-18T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 91.72222222222223,
          "Occur Date": "2012-12-19T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 91.87777777777778,
          "Occur Date": "2012-12-20T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 91.84444444444445,
          "Occur Date": "2012-12-21T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 92.11111111111111,
          "Occur Date": "2012-12-22T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 92.14444444444445,
          "Occur Date": "2012-12-23T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 92.35555555555555,
          "Occur Date": "2012-12-24T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 92,
          "Occur Date": "2012-12-25T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 91.75555555555556,
          "Occur Date": "2012-12-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 91.55555555555556,
          "Occur Date": "2012-12-27T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 91.44444444444444,
          "Occur Date": "2012-12-28T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 91.72222222222223,
          "Occur Date": "2012-12-29T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 91.7,
          "Occur Date": "2012-12-30T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 92.11111111111111,
          "Occur Date": "2012-12-31T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 92.35555555555555,
          "Occur Date": "2013-01-01T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 92.63333333333334,
          "Occur Date": "2013-01-02T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 92.18888888888888,
          "Occur Date": "2013-01-03T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 92.3,
          "Occur Date": "2013-01-04T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 92.25555555555556,
          "Occur Date": "2013-01-05T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 92.12222222222222,
          "Occur Date": "2013-01-06T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 92.32222222222222,
          "Occur Date": "2013-01-07T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 92.35555555555555,
          "Occur Date": "2013-01-08T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 92.38888888888889,
          "Occur Date": "2013-01-09T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 92,
          "Occur Date": "2013-01-10T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 91.74444444444444,
          "Occur Date": "2013-01-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 91.77777777777777,
          "Occur Date": "2013-01-12T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 91.56666666666666,
          "Occur Date": "2013-01-13T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 91.55555555555556,
          "Occur Date": "2013-01-14T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 91.71111111111111,
          "Occur Date": "2013-01-15T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 91.56666666666666,
          "Occur Date": "2013-01-16T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 91.61111111111111,
          "Occur Date": "2013-01-17T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 91.73333333333333,
          "Occur Date": "2013-01-18T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 92.05555555555556,
          "Occur Date": "2013-01-19T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 92.4888888888889,
          "Occur Date": "2013-01-20T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 92.25555555555556,
          "Occur Date": "2013-01-21T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 92.26666666666667,
          "Occur Date": "2013-01-22T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 91.95555555555555,
          "Occur Date": "2013-01-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 91.8,
          "Occur Date": "2013-01-24T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.87777777777778,
          "Occur Date": "2013-01-25T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 92.14444444444445,
          "Occur Date": "2013-01-26T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 92,
          "Occur Date": "2013-01-27T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 92.03333333333333,
          "Occur Date": "2013-01-28T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 91.94444444444444,
          "Occur Date": "2013-01-29T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 91.76666666666667,
          "Occur Date": "2013-01-30T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 91.66666666666667,
          "Occur Date": "2013-01-31T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 91.55555555555556,
          "Occur Date": "2013-02-01T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 91.4,
          "Occur Date": "2013-02-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 91.28888888888889,
          "Occur Date": "2013-02-03T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 91.15555555555555,
          "Occur Date": "2013-02-04T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.12222222222222,
          "Occur Date": "2013-02-05T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 90.83333333333333,
          "Occur Date": "2013-02-06T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 90.72222222222223,
          "Occur Date": "2013-02-07T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 90.62222222222222,
          "Occur Date": "2013-02-08T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 90.45555555555555,
          "Occur Date": "2013-02-09T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 90.03333333333333,
          "Occur Date": "2013-02-10T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 89.91111111111111,
          "Occur Date": "2013-02-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 89.41111111111111,
          "Occur Date": "2013-02-12T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 89.35555555555555,
          "Occur Date": "2013-02-13T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 89.06666666666666,
          "Occur Date": "2013-02-14T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 89.14444444444445,
          "Occur Date": "2013-02-15T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 88.9,
          "Occur Date": "2013-02-16T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 88.5111111111111,
          "Occur Date": "2013-02-17T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 88.4,
          "Occur Date": "2013-02-18T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 88.5111111111111,
          "Occur Date": "2013-02-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.62222222222222,
          "Occur Date": "2013-02-20T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 88.64444444444445,
          "Occur Date": "2013-02-21T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 88.8,
          "Occur Date": "2013-02-22T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 88.87777777777778,
          "Occur Date": "2013-02-23T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 88.57777777777778,
          "Occur Date": "2013-02-24T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 88.4888888888889,
          "Occur Date": "2013-02-25T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 88.17777777777778,
          "Occur Date": "2013-02-26T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 88.16666666666667,
          "Occur Date": "2013-02-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 87.97777777777777,
          "Occur Date": "2013-02-28T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 87.9888888888889,
          "Occur Date": "2013-03-01T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 87.61111111111111,
          "Occur Date": "2013-03-02T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 87.24444444444444,
          "Occur Date": "2013-03-03T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 87.12222222222222,
          "Occur Date": "2013-03-04T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 86.95555555555555,
          "Occur Date": "2013-03-05T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 86.62222222222222,
          "Occur Date": "2013-03-06T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 86.31111111111112,
          "Occur Date": "2013-03-07T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 86.18888888888888,
          "Occur Date": "2013-03-08T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 86.4888888888889,
          "Occur Date": "2013-03-09T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 86.02222222222223,
          "Occur Date": "2013-03-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 86.08888888888889,
          "Occur Date": "2013-03-11T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 86.04444444444445,
          "Occur Date": "2013-03-12T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2013-03-13T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 85.65555555555555,
          "Occur Date": "2013-03-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.41111111111111,
          "Occur Date": "2013-03-15T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 85.68888888888888,
          "Occur Date": "2013-03-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 85.45555555555555,
          "Occur Date": "2013-03-17T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 84.85555555555555,
          "Occur Date": "2013-03-18T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 84.64444444444445,
          "Occur Date": "2013-03-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.17777777777778,
          "Occur Date": "2013-03-20T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 84.1,
          "Occur Date": "2013-03-21T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 84.08888888888889,
          "Occur Date": "2013-03-22T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 83.95555555555555,
          "Occur Date": "2013-03-23T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 83.54444444444445,
          "Occur Date": "2013-03-24T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 83.66666666666667,
          "Occur Date": "2013-03-25T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 83.74444444444444,
          "Occur Date": "2013-03-26T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 83.55555555555556,
          "Occur Date": "2013-03-27T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 83.35555555555555,
          "Occur Date": "2013-03-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 83.26666666666667,
          "Occur Date": "2013-03-29T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 83.33333333333333,
          "Occur Date": "2013-03-30T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 82.76666666666667,
          "Occur Date": "2013-03-31T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 82.52222222222223,
          "Occur Date": "2013-04-01T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 82.12222222222222,
          "Occur Date": "2013-04-02T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 82.1,
          "Occur Date": "2013-04-03T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.64444444444445,
          "Occur Date": "2013-04-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 81.57777777777778,
          "Occur Date": "2013-04-05T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 82.06666666666666,
          "Occur Date": "2013-04-06T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 81.84444444444445,
          "Occur Date": "2013-04-07T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.81111111111112,
          "Occur Date": "2013-04-08T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 81.64444444444445,
          "Occur Date": "2013-04-09T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.62222222222222,
          "Occur Date": "2013-04-10T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.62222222222222,
          "Occur Date": "2013-04-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 81.58888888888889,
          "Occur Date": "2013-04-12T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 81.54444444444445,
          "Occur Date": "2013-04-13T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.36666666666666,
          "Occur Date": "2013-04-14T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 81.06666666666666,
          "Occur Date": "2013-04-15T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 81.3,
          "Occur Date": "2013-04-16T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 81.27777777777777,
          "Occur Date": "2013-04-17T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 81.04444444444445,
          "Occur Date": "2013-04-18T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 80.94444444444444,
          "Occur Date": "2013-04-19T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 80.58888888888889,
          "Occur Date": "2013-04-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 80.63333333333334,
          "Occur Date": "2013-04-21T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 80.75555555555556,
          "Occur Date": "2013-04-22T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 80.88888888888889,
          "Occur Date": "2013-04-23T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 81.0111111111111,
          "Occur Date": "2013-04-24T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 81.13333333333334,
          "Occur Date": "2013-04-25T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 80.93333333333334,
          "Occur Date": "2013-04-26T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 81.17777777777778,
          "Occur Date": "2013-04-27T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 80.68888888888888,
          "Occur Date": "2013-04-28T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 80.57777777777778,
          "Occur Date": "2013-04-29T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 80.64444444444445,
          "Occur Date": "2013-04-30T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 80.9,
          "Occur Date": "2013-05-01T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 80.87777777777778,
          "Occur Date": "2013-05-02T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.38888888888889,
          "Occur Date": "2013-05-03T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 81.65555555555555,
          "Occur Date": "2013-05-04T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 81.71111111111111,
          "Occur Date": "2013-05-05T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 81.78888888888889,
          "Occur Date": "2013-05-06T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 82.2,
          "Occur Date": "2013-05-07T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 82.44444444444444,
          "Occur Date": "2013-05-08T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 82.78888888888889,
          "Occur Date": "2013-05-09T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 83.25555555555556,
          "Occur Date": "2013-05-10T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 83.54444444444445,
          "Occur Date": "2013-05-11T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 83.44444444444444,
          "Occur Date": "2013-05-12T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 83.87777777777778,
          "Occur Date": "2013-05-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 83.91111111111111,
          "Occur Date": "2013-05-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 84.18888888888888,
          "Occur Date": "2013-05-15T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 84.13333333333334,
          "Occur Date": "2013-05-16T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.6,
          "Occur Date": "2013-05-17T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 84.9,
          "Occur Date": "2013-05-18T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 84.86666666666666,
          "Occur Date": "2013-05-19T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 84.9,
          "Occur Date": "2013-05-20T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 85.07777777777778,
          "Occur Date": "2013-05-21T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 85,
          "Occur Date": "2013-05-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 85.1,
          "Occur Date": "2013-05-23T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 85.04444444444445,
          "Occur Date": "2013-05-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 85.6,
          "Occur Date": "2013-05-25T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 85.74444444444444,
          "Occur Date": "2013-05-26T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.02222222222223,
          "Occur Date": "2013-05-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 86.28888888888889,
          "Occur Date": "2013-05-28T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 86.54444444444445,
          "Occur Date": "2013-05-29T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 86.38888888888889,
          "Occur Date": "2013-05-30T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 86.93333333333334,
          "Occur Date": "2013-05-31T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 87.58888888888889,
          "Occur Date": "2013-06-01T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 87.63333333333334,
          "Occur Date": "2013-06-02T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 87.82222222222222,
          "Occur Date": "2013-06-03T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 88.13333333333334,
          "Occur Date": "2013-06-04T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.43333333333334,
          "Occur Date": "2013-06-05T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.65555555555555,
          "Occur Date": "2013-06-06T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 88.84444444444445,
          "Occur Date": "2013-06-07T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 89.3,
          "Occur Date": "2013-06-08T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 89.14444444444445,
          "Occur Date": "2013-06-09T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 89.32222222222222,
          "Occur Date": "2013-06-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 89.35555555555555,
          "Occur Date": "2013-06-11T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 89.5111111111111,
          "Occur Date": "2013-06-12T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.64444444444445,
          "Occur Date": "2013-06-13T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.91111111111111,
          "Occur Date": "2013-06-14T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 90.24444444444444,
          "Occur Date": "2013-06-15T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 90.72222222222223,
          "Occur Date": "2013-06-16T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 90.7,
          "Occur Date": "2013-06-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 90.84444444444445,
          "Occur Date": "2013-06-18T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 91.08888888888889,
          "Occur Date": "2013-06-19T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 91.17777777777778,
          "Occur Date": "2013-06-20T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 91.34444444444445,
          "Occur Date": "2013-06-21T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.67777777777778,
          "Occur Date": "2013-06-22T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 92.0111111111111,
          "Occur Date": "2013-06-23T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 92.21111111111111,
          "Occur Date": "2013-06-24T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 92.6,
          "Occur Date": "2013-06-25T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 92.62222222222222,
          "Occur Date": "2013-06-26T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 92.5111111111111,
          "Occur Date": "2013-06-27T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 92.82222222222222,
          "Occur Date": "2013-06-28T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 93.18888888888888,
          "Occur Date": "2013-06-29T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 93.15555555555555,
          "Occur Date": "2013-06-30T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 93.63333333333334,
          "Occur Date": "2013-07-01T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 93.67777777777778,
          "Occur Date": "2013-07-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 93.9,
          "Occur Date": "2013-07-03T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 93.75555555555556,
          "Occur Date": "2013-07-04T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 93.53333333333333,
          "Occur Date": "2013-07-05T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 93.53333333333333,
          "Occur Date": "2013-07-06T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 93.46666666666667,
          "Occur Date": "2013-07-07T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 93.56666666666666,
          "Occur Date": "2013-07-08T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 93.64444444444445,
          "Occur Date": "2013-07-09T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 93.7,
          "Occur Date": "2013-07-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 93.56666666666666,
          "Occur Date": "2013-07-11T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 93.93333333333334,
          "Occur Date": "2013-07-12T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 94.5111111111111,
          "Occur Date": "2013-07-13T00:00:00",
          "Volume": 118
         },
         {
          "90-day": 94.87777777777778,
          "Occur Date": "2013-07-14T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 94.9,
          "Occur Date": "2013-07-15T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 94.93333333333334,
          "Occur Date": "2013-07-16T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 95.07777777777778,
          "Occur Date": "2013-07-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 95.25555555555556,
          "Occur Date": "2013-07-18T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 95.37777777777778,
          "Occur Date": "2013-07-19T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 95.52222222222223,
          "Occur Date": "2013-07-20T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 95.33333333333333,
          "Occur Date": "2013-07-21T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 95.58888888888889,
          "Occur Date": "2013-07-22T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 95.53333333333333,
          "Occur Date": "2013-07-23T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 95.72222222222223,
          "Occur Date": "2013-07-24T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 95.94444444444444,
          "Occur Date": "2013-07-25T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 96,
          "Occur Date": "2013-07-26T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 96.46666666666667,
          "Occur Date": "2013-07-27T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 96.4888888888889,
          "Occur Date": "2013-07-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 96.58888888888889,
          "Occur Date": "2013-07-29T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 96.6,
          "Occur Date": "2013-07-30T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 96.65555555555555,
          "Occur Date": "2013-07-31T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 96.43333333333334,
          "Occur Date": "2013-08-01T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 96.31111111111112,
          "Occur Date": "2013-08-02T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 96.45555555555555,
          "Occur Date": "2013-08-03T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 96.35555555555555,
          "Occur Date": "2013-08-04T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 96.27777777777777,
          "Occur Date": "2013-08-05T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 96.12222222222222,
          "Occur Date": "2013-08-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 95.9,
          "Occur Date": "2013-08-07T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 95.83333333333333,
          "Occur Date": "2013-08-08T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 96.21111111111111,
          "Occur Date": "2013-08-09T00:00:00",
          "Volume": 114
         },
         {
          "90-day": 96.3,
          "Occur Date": "2013-08-10T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 96.04444444444445,
          "Occur Date": "2013-08-11T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 95.9,
          "Occur Date": "2013-08-12T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 95.74444444444444,
          "Occur Date": "2013-08-13T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.91111111111111,
          "Occur Date": "2013-08-14T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 95.77777777777777,
          "Occur Date": "2013-08-15T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 95.74444444444444,
          "Occur Date": "2013-08-16T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 95.85555555555555,
          "Occur Date": "2013-08-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.81111111111112,
          "Occur Date": "2013-08-18T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 95.82222222222222,
          "Occur Date": "2013-08-19T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 96.1,
          "Occur Date": "2013-08-20T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 95.7,
          "Occur Date": "2013-08-21T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 95.68888888888888,
          "Occur Date": "2013-08-22T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.5111111111111,
          "Occur Date": "2013-08-23T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 95.76666666666667,
          "Occur Date": "2013-08-24T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 95.63333333333334,
          "Occur Date": "2013-08-25T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 95.45555555555555,
          "Occur Date": "2013-08-26T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.52222222222223,
          "Occur Date": "2013-08-27T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 95.44444444444444,
          "Occur Date": "2013-08-28T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 95.28888888888889,
          "Occur Date": "2013-08-29T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 95.31111111111112,
          "Occur Date": "2013-08-30T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 95.71111111111111,
          "Occur Date": "2013-08-31T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 95.77777777777777,
          "Occur Date": "2013-09-01T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 95.91111111111111,
          "Occur Date": "2013-09-02T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 95.86666666666666,
          "Occur Date": "2013-09-03T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.68888888888888,
          "Occur Date": "2013-09-04T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.43333333333334,
          "Occur Date": "2013-09-05T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.41111111111111,
          "Occur Date": "2013-09-06T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 95.82222222222222,
          "Occur Date": "2013-09-07T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 95.5111111111111,
          "Occur Date": "2013-09-08T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 95.57777777777778,
          "Occur Date": "2013-09-09T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 95.58888888888889,
          "Occur Date": "2013-09-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 95.66666666666667,
          "Occur Date": "2013-09-11T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 95.36666666666666,
          "Occur Date": "2013-09-12T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 95.44444444444444,
          "Occur Date": "2013-09-13T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 95.34444444444445,
          "Occur Date": "2013-09-14T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 95.33333333333333,
          "Occur Date": "2013-09-15T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 95.38888888888889,
          "Occur Date": "2013-09-16T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 95.12222222222222,
          "Occur Date": "2013-09-17T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 94.72222222222223,
          "Occur Date": "2013-09-18T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 94.82222222222222,
          "Occur Date": "2013-09-19T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 94.97777777777777,
          "Occur Date": "2013-09-20T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 94.88888888888889,
          "Occur Date": "2013-09-21T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 94.68888888888888,
          "Occur Date": "2013-09-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 94.47777777777777,
          "Occur Date": "2013-09-23T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 94.66666666666667,
          "Occur Date": "2013-09-24T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 94.74444444444444,
          "Occur Date": "2013-09-25T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 94.46666666666667,
          "Occur Date": "2013-09-26T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 94.53333333333333,
          "Occur Date": "2013-09-27T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 94.94444444444444,
          "Occur Date": "2013-09-28T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 94.72222222222223,
          "Occur Date": "2013-09-29T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 94.63333333333334,
          "Occur Date": "2013-09-30T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 94.76666666666667,
          "Occur Date": "2013-10-01T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 94.76666666666667,
          "Occur Date": "2013-10-02T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 94.62222222222222,
          "Occur Date": "2013-10-03T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 94.75555555555556,
          "Occur Date": "2013-10-04T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 94.85555555555555,
          "Occur Date": "2013-10-05T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 94.65555555555555,
          "Occur Date": "2013-10-06T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 94.72222222222223,
          "Occur Date": "2013-10-07T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 94.55555555555556,
          "Occur Date": "2013-10-08T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 94.64444444444445,
          "Occur Date": "2013-10-09T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 94.1,
          "Occur Date": "2013-10-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 93.84444444444445,
          "Occur Date": "2013-10-11T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 93.9,
          "Occur Date": "2013-10-12T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 93.63333333333334,
          "Occur Date": "2013-10-13T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 93.42222222222222,
          "Occur Date": "2013-10-14T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 93.56666666666666,
          "Occur Date": "2013-10-15T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 93.25555555555556,
          "Occur Date": "2013-10-16T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 93.11111111111111,
          "Occur Date": "2013-10-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 93.17777777777778,
          "Occur Date": "2013-10-18T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 93.03333333333333,
          "Occur Date": "2013-10-19T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 92.45555555555555,
          "Occur Date": "2013-10-20T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 92.38888888888889,
          "Occur Date": "2013-10-21T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.84444444444445,
          "Occur Date": "2013-10-22T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 91.5,
          "Occur Date": "2013-10-23T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 91.23333333333333,
          "Occur Date": "2013-10-24T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 91.13333333333334,
          "Occur Date": "2013-10-25T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 91.33333333333333,
          "Occur Date": "2013-10-26T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 91.17777777777778,
          "Occur Date": "2013-10-27T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 90.91111111111111,
          "Occur Date": "2013-10-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90.85555555555555,
          "Occur Date": "2013-10-29T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90.71111111111111,
          "Occur Date": "2013-10-30T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.61111111111111,
          "Occur Date": "2013-10-31T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 90.67777777777778,
          "Occur Date": "2013-11-01T00:00:00",
          "Volume": 112
         },
         {
          "90-day": 90.67777777777778,
          "Occur Date": "2013-11-02T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 90.24444444444444,
          "Occur Date": "2013-11-03T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 89.96666666666667,
          "Occur Date": "2013-11-04T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 89.96666666666667,
          "Occur Date": "2013-11-05T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 89.75555555555556,
          "Occur Date": "2013-11-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 89.32222222222222,
          "Occur Date": "2013-11-07T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 89.3,
          "Occur Date": "2013-11-08T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.34444444444445,
          "Occur Date": "2013-11-09T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 89.25555555555556,
          "Occur Date": "2013-11-10T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 89.33333333333333,
          "Occur Date": "2013-11-11T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 89.1,
          "Occur Date": "2013-11-12T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 88.78888888888889,
          "Occur Date": "2013-11-13T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 88.61111111111111,
          "Occur Date": "2013-11-14T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2013-11-15T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 88.58888888888889,
          "Occur Date": "2013-11-16T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 88.55555555555556,
          "Occur Date": "2013-11-17T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 88.3,
          "Occur Date": "2013-11-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.23333333333333,
          "Occur Date": "2013-11-19T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 88.36666666666666,
          "Occur Date": "2013-11-20T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 88.28888888888889,
          "Occur Date": "2013-11-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.16666666666667,
          "Occur Date": "2013-11-22T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.15555555555555,
          "Occur Date": "2013-11-23T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 88.12222222222222,
          "Occur Date": "2013-11-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 87.97777777777777,
          "Occur Date": "2013-11-25T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.1,
          "Occur Date": "2013-11-26T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 87.94444444444444,
          "Occur Date": "2013-11-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 87.38888888888889,
          "Occur Date": "2013-11-28T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 87.08888888888889,
          "Occur Date": "2013-11-29T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 86.93333333333334,
          "Occur Date": "2013-11-30T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 86.86666666666666,
          "Occur Date": "2013-12-01T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 86.85555555555555,
          "Occur Date": "2013-12-02T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 86.75555555555556,
          "Occur Date": "2013-12-03T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 86.57777777777778,
          "Occur Date": "2013-12-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 86.43333333333334,
          "Occur Date": "2013-12-05T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 86.3,
          "Occur Date": "2013-12-06T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 86.53333333333333,
          "Occur Date": "2013-12-07T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 86.45555555555555,
          "Occur Date": "2013-12-08T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 86.42222222222222,
          "Occur Date": "2013-12-09T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 86.06666666666666,
          "Occur Date": "2013-12-10T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 85.77777777777777,
          "Occur Date": "2013-12-11T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 85.5,
          "Occur Date": "2013-12-12T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 85.55555555555556,
          "Occur Date": "2013-12-13T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2013-12-14T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 85.77777777777777,
          "Occur Date": "2013-12-15T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 85.74444444444444,
          "Occur Date": "2013-12-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 85.9,
          "Occur Date": "2013-12-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 85.76666666666667,
          "Occur Date": "2013-12-18T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.34444444444445,
          "Occur Date": "2013-12-19T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 85.68888888888888,
          "Occur Date": "2013-12-20T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 85.93333333333334,
          "Occur Date": "2013-12-21T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.85555555555555,
          "Occur Date": "2013-12-22T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 85.92222222222222,
          "Occur Date": "2013-12-23T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2013-12-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 85.24444444444444,
          "Occur Date": "2013-12-25T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 84.96666666666667,
          "Occur Date": "2013-12-26T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 84.47777777777777,
          "Occur Date": "2013-12-27T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 84.16666666666667,
          "Occur Date": "2013-12-28T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 84.07777777777778,
          "Occur Date": "2013-12-29T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 83.74444444444444,
          "Occur Date": "2013-12-30T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 84.05555555555556,
          "Occur Date": "2013-12-31T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 84.12222222222222,
          "Occur Date": "2014-01-01T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 83.87777777777778,
          "Occur Date": "2014-01-02T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2014-01-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 83.76666666666667,
          "Occur Date": "2014-01-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 83.54444444444445,
          "Occur Date": "2014-01-05T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 83.36666666666666,
          "Occur Date": "2014-01-06T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 83.06666666666666,
          "Occur Date": "2014-01-07T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2014-01-08T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 83.24444444444444,
          "Occur Date": "2014-01-09T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2014-01-10T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2014-01-11T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 83.22222222222223,
          "Occur Date": "2014-01-12T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 82.9888888888889,
          "Occur Date": "2014-01-13T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 83.1,
          "Occur Date": "2014-01-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 82.97777777777777,
          "Occur Date": "2014-01-15T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 82.72222222222223,
          "Occur Date": "2014-01-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.06666666666666,
          "Occur Date": "2014-01-17T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 83.32222222222222,
          "Occur Date": "2014-01-18T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 83.34444444444445,
          "Occur Date": "2014-01-19T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 83.33333333333333,
          "Occur Date": "2014-01-20T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 83.36666666666666,
          "Occur Date": "2014-01-21T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 83.12222222222222,
          "Occur Date": "2014-01-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 82.82222222222222,
          "Occur Date": "2014-01-23T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 82.4,
          "Occur Date": "2014-01-24T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 82.52222222222223,
          "Occur Date": "2014-01-25T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 82.4,
          "Occur Date": "2014-01-26T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 82.22222222222223,
          "Occur Date": "2014-01-27T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 82.11111111111111,
          "Occur Date": "2014-01-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.55555555555556,
          "Occur Date": "2014-01-29T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 80.88888888888889,
          "Occur Date": "2014-01-30T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 80.93333333333334,
          "Occur Date": "2014-01-31T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 81.25555555555556,
          "Occur Date": "2014-02-01T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 81.41111111111111,
          "Occur Date": "2014-02-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 81.3,
          "Occur Date": "2014-02-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 81.18888888888888,
          "Occur Date": "2014-02-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 81.21111111111111,
          "Occur Date": "2014-02-05T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 81.24444444444444,
          "Occur Date": "2014-02-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 81.38888888888889,
          "Occur Date": "2014-02-07T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 81.53333333333333,
          "Occur Date": "2014-02-08T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 81,
          "Occur Date": "2014-02-09T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 81.1,
          "Occur Date": "2014-02-10T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 81.03333333333333,
          "Occur Date": "2014-02-11T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 80.5,
          "Occur Date": "2014-02-12T00:00:00",
          "Volume": 28
         },
         {
          "90-day": 79.93333333333334,
          "Occur Date": "2014-02-13T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2014-02-14T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 79.8,
          "Occur Date": "2014-02-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.64444444444445,
          "Occur Date": "2014-02-16T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 79.6,
          "Occur Date": "2014-02-17T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 79.37777777777778,
          "Occur Date": "2014-02-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.08888888888889,
          "Occur Date": "2014-02-19T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 79.07777777777778,
          "Occur Date": "2014-02-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 78.96666666666667,
          "Occur Date": "2014-02-21T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 78.97777777777777,
          "Occur Date": "2014-02-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 78.67777777777778,
          "Occur Date": "2014-02-23T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 78.28888888888889,
          "Occur Date": "2014-02-24T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 78.26666666666667,
          "Occur Date": "2014-02-25T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 78.44444444444444,
          "Occur Date": "2014-02-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 77.9,
          "Occur Date": "2014-02-27T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 77.8,
          "Occur Date": "2014-02-28T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 77.42222222222222,
          "Occur Date": "2014-03-01T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 77.06666666666666,
          "Occur Date": "2014-03-02T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 76.93333333333334,
          "Occur Date": "2014-03-03T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 76.84444444444445,
          "Occur Date": "2014-03-04T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 76.67777777777778,
          "Occur Date": "2014-03-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 76.21111111111111,
          "Occur Date": "2014-03-06T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 75.76666666666667,
          "Occur Date": "2014-03-07T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 75.62222222222222,
          "Occur Date": "2014-03-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 75.33333333333333,
          "Occur Date": "2014-03-09T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 75.34444444444445,
          "Occur Date": "2014-03-10T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 75.45555555555555,
          "Occur Date": "2014-03-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 75.3,
          "Occur Date": "2014-03-12T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 75.05555555555556,
          "Occur Date": "2014-03-13T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.47777777777777,
          "Occur Date": "2014-03-14T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 74.55555555555556,
          "Occur Date": "2014-03-15T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 74.46666666666667,
          "Occur Date": "2014-03-16T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2014-03-17T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.34444444444445,
          "Occur Date": "2014-03-18T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 74.24444444444444,
          "Occur Date": "2014-03-19T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.64444444444445,
          "Occur Date": "2014-03-20T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.44444444444444,
          "Occur Date": "2014-03-21T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.52222222222223,
          "Occur Date": "2014-03-22T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.08888888888889,
          "Occur Date": "2014-03-23T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.04444444444445,
          "Occur Date": "2014-03-24T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.4888888888889,
          "Occur Date": "2014-03-25T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2014-03-26T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2014-03-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.73333333333333,
          "Occur Date": "2014-03-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.85555555555555,
          "Occur Date": "2014-03-29T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 73.92222222222222,
          "Occur Date": "2014-03-30T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.5,
          "Occur Date": "2014-03-31T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2014-04-01T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2014-04-02T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2014-04-03T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2014-04-04T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 73.62222222222222,
          "Occur Date": "2014-04-05T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.78888888888889,
          "Occur Date": "2014-04-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 74.14444444444445,
          "Occur Date": "2014-04-07T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 74.21111111111111,
          "Occur Date": "2014-04-08T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 73.94444444444444,
          "Occur Date": "2014-04-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 73.92222222222222,
          "Occur Date": "2014-04-10T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 74.02222222222223,
          "Occur Date": "2014-04-11T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 74.06666666666666,
          "Occur Date": "2014-04-12T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.87777777777778,
          "Occur Date": "2014-04-13T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 73.84444444444445,
          "Occur Date": "2014-04-14T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 74.0111111111111,
          "Occur Date": "2014-04-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 73.9888888888889,
          "Occur Date": "2014-04-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.85555555555555,
          "Occur Date": "2014-04-17T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 73.91111111111111,
          "Occur Date": "2014-04-18T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 73.74444444444444,
          "Occur Date": "2014-04-19T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.63333333333334,
          "Occur Date": "2014-04-20T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.64444444444445,
          "Occur Date": "2014-04-21T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.94444444444444,
          "Occur Date": "2014-04-22T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 74.25555555555556,
          "Occur Date": "2014-04-23T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2014-04-24T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 74.3,
          "Occur Date": "2014-04-25T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 74.53333333333333,
          "Occur Date": "2014-04-26T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 74.52222222222223,
          "Occur Date": "2014-04-27T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 74.62222222222222,
          "Occur Date": "2014-04-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 75.12222222222222,
          "Occur Date": "2014-04-29T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 75.38888888888889,
          "Occur Date": "2014-04-30T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 75.41111111111111,
          "Occur Date": "2014-05-01T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 75.26666666666667,
          "Occur Date": "2014-05-02T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 75.41111111111111,
          "Occur Date": "2014-05-03T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 75.33333333333333,
          "Occur Date": "2014-05-04T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 75.37777777777778,
          "Occur Date": "2014-05-05T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 75.38888888888889,
          "Occur Date": "2014-05-06T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 75.36666666666666,
          "Occur Date": "2014-05-07T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 75.31111111111112,
          "Occur Date": "2014-05-08T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 75.31111111111112,
          "Occur Date": "2014-05-09T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 75.82222222222222,
          "Occur Date": "2014-05-10T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 75.58888888888889,
          "Occur Date": "2014-05-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 75.81111111111112,
          "Occur Date": "2014-05-12T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 76.38888888888889,
          "Occur Date": "2014-05-13T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 76.74444444444444,
          "Occur Date": "2014-05-14T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 77.05555555555556,
          "Occur Date": "2014-05-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 77.08888888888889,
          "Occur Date": "2014-05-16T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 77.14444444444445,
          "Occur Date": "2014-05-17T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 77.18888888888888,
          "Occur Date": "2014-05-18T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 77.2,
          "Occur Date": "2014-05-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 77.33333333333333,
          "Occur Date": "2014-05-20T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 77.26666666666667,
          "Occur Date": "2014-05-21T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 77.27777777777777,
          "Occur Date": "2014-05-22T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 77.4888888888889,
          "Occur Date": "2014-05-23T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 77.8,
          "Occur Date": "2014-05-24T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 77.91111111111111,
          "Occur Date": "2014-05-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 77.95555555555555,
          "Occur Date": "2014-05-26T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 77.9888888888889,
          "Occur Date": "2014-05-27T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 78.56666666666666,
          "Occur Date": "2014-05-28T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 78.66666666666667,
          "Occur Date": "2014-05-29T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 78.85555555555555,
          "Occur Date": "2014-05-30T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 79.24444444444444,
          "Occur Date": "2014-05-31T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 79.5,
          "Occur Date": "2014-06-01T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 79.76666666666667,
          "Occur Date": "2014-06-02T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 79.73333333333333,
          "Occur Date": "2014-06-03T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 80.03333333333333,
          "Occur Date": "2014-06-04T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 80.38888888888889,
          "Occur Date": "2014-06-05T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 80.56666666666666,
          "Occur Date": "2014-06-06T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.06666666666666,
          "Occur Date": "2014-06-07T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 81.16666666666667,
          "Occur Date": "2014-06-08T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 81.42222222222222,
          "Occur Date": "2014-06-09T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 81.55555555555556,
          "Occur Date": "2014-06-10T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 81.58888888888889,
          "Occur Date": "2014-06-11T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 81.95555555555555,
          "Occur Date": "2014-06-12T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 82.1,
          "Occur Date": "2014-06-13T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 82.72222222222223,
          "Occur Date": "2014-06-14T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 82.91111111111111,
          "Occur Date": "2014-06-15T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 82.9,
          "Occur Date": "2014-06-16T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 83.16666666666667,
          "Occur Date": "2014-06-17T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 83.3,
          "Occur Date": "2014-06-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.44444444444444,
          "Occur Date": "2014-06-19T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 83.54444444444445,
          "Occur Date": "2014-06-20T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 83.66666666666667,
          "Occur Date": "2014-06-21T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.86666666666666,
          "Occur Date": "2014-06-22T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 84.02222222222223,
          "Occur Date": "2014-06-23T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 84.13333333333334,
          "Occur Date": "2014-06-24T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 84.08888888888889,
          "Occur Date": "2014-06-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 84.08888888888889,
          "Occur Date": "2014-06-26T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 84.22222222222223,
          "Occur Date": "2014-06-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 84.44444444444444,
          "Occur Date": "2014-06-28T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 84.7,
          "Occur Date": "2014-06-29T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.91111111111111,
          "Occur Date": "2014-06-30T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 85.28888888888889,
          "Occur Date": "2014-07-01T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 85.61111111111111,
          "Occur Date": "2014-07-02T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.8,
          "Occur Date": "2014-07-03T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 85.8,
          "Occur Date": "2014-07-04T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 85.88888888888889,
          "Occur Date": "2014-07-05T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2014-07-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 85.93333333333334,
          "Occur Date": "2014-07-07T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 86.04444444444445,
          "Occur Date": "2014-07-08T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2014-07-09T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 85.95555555555555,
          "Occur Date": "2014-07-10T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 86.1,
          "Occur Date": "2014-07-11T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 86.62222222222222,
          "Occur Date": "2014-07-12T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 86.7,
          "Occur Date": "2014-07-13T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 86.92222222222222,
          "Occur Date": "2014-07-14T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 87.44444444444444,
          "Occur Date": "2014-07-15T00:00:00",
          "Volume": 122
         },
         {
          "90-day": 87.62222222222222,
          "Occur Date": "2014-07-16T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 87.46666666666667,
          "Occur Date": "2014-07-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 87.56666666666666,
          "Occur Date": "2014-07-18T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 87.8,
          "Occur Date": "2014-07-19T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 87.9888888888889,
          "Occur Date": "2014-07-20T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 88.03333333333333,
          "Occur Date": "2014-07-21T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 88.08888888888889,
          "Occur Date": "2014-07-22T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.12222222222222,
          "Occur Date": "2014-07-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 88.31111111111112,
          "Occur Date": "2014-07-24T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 88.26666666666667,
          "Occur Date": "2014-07-25T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 88.5,
          "Occur Date": "2014-07-26T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 88.5,
          "Occur Date": "2014-07-27T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.5111111111111,
          "Occur Date": "2014-07-28T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.38888888888889,
          "Occur Date": "2014-07-29T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 88.24444444444444,
          "Occur Date": "2014-07-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 88.37777777777778,
          "Occur Date": "2014-07-31T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.66666666666667,
          "Occur Date": "2014-08-01T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 89.03333333333333,
          "Occur Date": "2014-08-02T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 89.03333333333333,
          "Occur Date": "2014-08-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 89.08888888888889,
          "Occur Date": "2014-08-04T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 89.0111111111111,
          "Occur Date": "2014-08-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 88.91111111111111,
          "Occur Date": "2014-08-06T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 88.93333333333334,
          "Occur Date": "2014-08-07T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 88.78888888888889,
          "Occur Date": "2014-08-08T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.05555555555556,
          "Occur Date": "2014-08-09T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.96666666666667,
          "Occur Date": "2014-08-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 89.02222222222223,
          "Occur Date": "2014-08-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 89.18888888888888,
          "Occur Date": "2014-08-12T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.13333333333334,
          "Occur Date": "2014-08-13T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 89.3,
          "Occur Date": "2014-08-14T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 89.6,
          "Occur Date": "2014-08-15T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 89.71111111111111,
          "Occur Date": "2014-08-16T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.84444444444445,
          "Occur Date": "2014-08-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.05555555555556,
          "Occur Date": "2014-08-18T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 90.0111111111111,
          "Occur Date": "2014-08-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 90.26666666666667,
          "Occur Date": "2014-08-20T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 90.28888888888889,
          "Occur Date": "2014-08-21T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 90.26666666666667,
          "Occur Date": "2014-08-22T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 90.4888888888889,
          "Occur Date": "2014-08-23T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 90.4888888888889,
          "Occur Date": "2014-08-24T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 90.43333333333334,
          "Occur Date": "2014-08-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 90.45555555555555,
          "Occur Date": "2014-08-26T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.36666666666666,
          "Occur Date": "2014-08-27T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 90.46666666666667,
          "Occur Date": "2014-08-28T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 90.45555555555555,
          "Occur Date": "2014-08-29T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 90.58888888888889,
          "Occur Date": "2014-08-30T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 90.56666666666666,
          "Occur Date": "2014-08-31T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 90.9888888888889,
          "Occur Date": "2014-09-01T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 91,
          "Occur Date": "2014-09-02T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 91.0111111111111,
          "Occur Date": "2014-09-03T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 91.12222222222222,
          "Occur Date": "2014-09-04T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 90.67777777777778,
          "Occur Date": "2014-09-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 90.95555555555555,
          "Occur Date": "2014-09-06T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 90.84444444444445,
          "Occur Date": "2014-09-07T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 90.78888888888889,
          "Occur Date": "2014-09-08T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 90.93333333333334,
          "Occur Date": "2014-09-09T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 91.06666666666666,
          "Occur Date": "2014-09-10T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 90.78888888888889,
          "Occur Date": "2014-09-11T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 90.34444444444445,
          "Occur Date": "2014-09-12T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 90.23333333333333,
          "Occur Date": "2014-09-13T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 90.32222222222222,
          "Occur Date": "2014-09-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90.14444444444445,
          "Occur Date": "2014-09-15T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 90.22222222222223,
          "Occur Date": "2014-09-16T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 89.97777777777777,
          "Occur Date": "2014-09-17T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 89.71111111111111,
          "Occur Date": "2014-09-18T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 90.03333333333333,
          "Occur Date": "2014-09-19T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 90.26666666666667,
          "Occur Date": "2014-09-20T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 90.23333333333333,
          "Occur Date": "2014-09-21T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 90.27777777777777,
          "Occur Date": "2014-09-22T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 90.4,
          "Occur Date": "2014-09-23T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 90.4,
          "Occur Date": "2014-09-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 90.21111111111111,
          "Occur Date": "2014-09-25T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 90.05555555555556,
          "Occur Date": "2014-09-26T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.15555555555555,
          "Occur Date": "2014-09-27T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 90.2,
          "Occur Date": "2014-09-28T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 89.83333333333333,
          "Occur Date": "2014-09-29T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 89.56666666666666,
          "Occur Date": "2014-09-30T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 89.57777777777778,
          "Occur Date": "2014-10-01T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 89.58888888888889,
          "Occur Date": "2014-10-02T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 89.66666666666667,
          "Occur Date": "2014-10-03T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.68888888888888,
          "Occur Date": "2014-10-04T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 89.21111111111111,
          "Occur Date": "2014-10-05T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 89.18888888888888,
          "Occur Date": "2014-10-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 89.14444444444445,
          "Occur Date": "2014-10-07T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 89.08888888888889,
          "Occur Date": "2014-10-08T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 89.04444444444445,
          "Occur Date": "2014-10-09T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 89.06666666666666,
          "Occur Date": "2014-10-10T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 89.22222222222223,
          "Occur Date": "2014-10-11T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 89.07777777777778,
          "Occur Date": "2014-10-12T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 88.76666666666667,
          "Occur Date": "2014-10-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.53333333333333,
          "Occur Date": "2014-10-14T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 88.61111111111111,
          "Occur Date": "2014-10-15T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.56666666666666,
          "Occur Date": "2014-10-16T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 88.58888888888889,
          "Occur Date": "2014-10-17T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.42222222222222,
          "Occur Date": "2014-10-18T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 88.27777777777777,
          "Occur Date": "2014-10-19T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 88.25555555555556,
          "Occur Date": "2014-10-20T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 88.24444444444444,
          "Occur Date": "2014-10-21T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 87.93333333333334,
          "Occur Date": "2014-10-22T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 87.76666666666667,
          "Occur Date": "2014-10-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 87.9888888888889,
          "Occur Date": "2014-10-24T00:00:00",
          "Volume": 113
         },
         {
          "90-day": 87.85555555555555,
          "Occur Date": "2014-10-25T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 87.67777777777778,
          "Occur Date": "2014-10-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 87.96666666666667,
          "Occur Date": "2014-10-27T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 88.06666666666666,
          "Occur Date": "2014-10-28T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.3,
          "Occur Date": "2014-10-29T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 87.91111111111111,
          "Occur Date": "2014-10-30T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.76666666666667,
          "Occur Date": "2014-10-31T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 88.23333333333333,
          "Occur Date": "2014-11-01T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 88.16666666666667,
          "Occur Date": "2014-11-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.3,
          "Occur Date": "2014-11-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.5,
          "Occur Date": "2014-11-04T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 88.6,
          "Occur Date": "2014-11-05T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 88.63333333333334,
          "Occur Date": "2014-11-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2014-11-07T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 89.34444444444445,
          "Occur Date": "2014-11-08T00:00:00",
          "Volume": 126
         },
         {
          "90-day": 89.4888888888889,
          "Occur Date": "2014-11-09T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 89.47777777777777,
          "Occur Date": "2014-11-10T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 89.41111111111111,
          "Occur Date": "2014-11-11T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 89.64444444444445,
          "Occur Date": "2014-11-12T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 89.58888888888889,
          "Occur Date": "2014-11-13T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 89.66666666666667,
          "Occur Date": "2014-11-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90,
          "Occur Date": "2014-11-15T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 89.84444444444445,
          "Occur Date": "2014-11-16T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 89.91111111111111,
          "Occur Date": "2014-11-17T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 89.9888888888889,
          "Occur Date": "2014-11-18T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 89.81111111111112,
          "Occur Date": "2014-11-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 89.94444444444444,
          "Occur Date": "2014-11-20T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.92222222222222,
          "Occur Date": "2014-11-21T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 90.32222222222222,
          "Occur Date": "2014-11-22T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 90.58888888888889,
          "Occur Date": "2014-11-23T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 90.66666666666667,
          "Occur Date": "2014-11-24T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 90.84444444444445,
          "Occur Date": "2014-11-25T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 91.07777777777778,
          "Occur Date": "2014-11-26T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 90.73333333333333,
          "Occur Date": "2014-11-27T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 90.63333333333334,
          "Occur Date": "2014-11-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.4888888888889,
          "Occur Date": "2014-11-29T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 90.24444444444444,
          "Occur Date": "2014-11-30T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 90.21111111111111,
          "Occur Date": "2014-12-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 90.37777777777778,
          "Occur Date": "2014-12-02T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 90.28888888888889,
          "Occur Date": "2014-12-03T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 90.28888888888889,
          "Occur Date": "2014-12-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 90.36666666666666,
          "Occur Date": "2014-12-05T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 90.74444444444444,
          "Occur Date": "2014-12-06T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 90.42222222222222,
          "Occur Date": "2014-12-07T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 90.33333333333333,
          "Occur Date": "2014-12-08T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 90.16666666666667,
          "Occur Date": "2014-12-09T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90.44444444444444,
          "Occur Date": "2014-12-10T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 90.62222222222222,
          "Occur Date": "2014-12-11T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 91.2,
          "Occur Date": "2014-12-12T00:00:00",
          "Volume": 134
         },
         {
          "90-day": 91.5,
          "Occur Date": "2014-12-13T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 91.54444444444445,
          "Occur Date": "2014-12-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 91.76666666666667,
          "Occur Date": "2014-12-15T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 92.41111111111111,
          "Occur Date": "2014-12-16T00:00:00",
          "Volume": 128
         },
         {
          "90-day": 92.55555555555556,
          "Occur Date": "2014-12-17T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 92.41111111111111,
          "Occur Date": "2014-12-18T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 92.2,
          "Occur Date": "2014-12-19T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 92.2,
          "Occur Date": "2014-12-20T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 92.26666666666667,
          "Occur Date": "2014-12-21T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 92.4,
          "Occur Date": "2014-12-22T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 92.6,
          "Occur Date": "2014-12-23T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 92.77777777777777,
          "Occur Date": "2014-12-24T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 92.25555555555556,
          "Occur Date": "2014-12-25T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 92.1,
          "Occur Date": "2014-12-26T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 92.02222222222223,
          "Occur Date": "2014-12-27T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 92.05555555555556,
          "Occur Date": "2014-12-28T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 92.17777777777778,
          "Occur Date": "2014-12-29T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 92.16666666666667,
          "Occur Date": "2014-12-30T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 92.4,
          "Occur Date": "2014-12-31T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 92.26666666666667,
          "Occur Date": "2015-01-01T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 92.23333333333333,
          "Occur Date": "2015-01-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 92.5111111111111,
          "Occur Date": "2015-01-03T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 92.47777777777777,
          "Occur Date": "2015-01-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 92.55555555555556,
          "Occur Date": "2015-01-05T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 92.33333333333333,
          "Occur Date": "2015-01-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 91.92222222222222,
          "Occur Date": "2015-01-07T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 91.37777777777778,
          "Occur Date": "2015-01-08T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 91.23333333333333,
          "Occur Date": "2015-01-09T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 90.94444444444444,
          "Occur Date": "2015-01-10T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 90.81111111111112,
          "Occur Date": "2015-01-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 90.95555555555555,
          "Occur Date": "2015-01-12T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 90.73333333333333,
          "Occur Date": "2015-01-13T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 90.65555555555555,
          "Occur Date": "2015-01-14T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 90.85555555555555,
          "Occur Date": "2015-01-15T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 90.9,
          "Occur Date": "2015-01-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 91.02222222222223,
          "Occur Date": "2015-01-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.87777777777778,
          "Occur Date": "2015-01-18T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 90.9888888888889,
          "Occur Date": "2015-01-19T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 90.96666666666667,
          "Occur Date": "2015-01-20T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 90.91111111111111,
          "Occur Date": "2015-01-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 90.54444444444445,
          "Occur Date": "2015-01-22T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 90.41111111111111,
          "Occur Date": "2015-01-23T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 90.73333333333333,
          "Occur Date": "2015-01-24T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 90.5111111111111,
          "Occur Date": "2015-01-25T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 90.53333333333333,
          "Occur Date": "2015-01-26T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 90.21111111111111,
          "Occur Date": "2015-01-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 90.31111111111112,
          "Occur Date": "2015-01-28T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 90.21111111111111,
          "Occur Date": "2015-01-29T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.71111111111111,
          "Occur Date": "2015-01-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 89.85555555555555,
          "Occur Date": "2015-01-31T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 89.75555555555556,
          "Occur Date": "2015-02-01T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 89.64444444444445,
          "Occur Date": "2015-02-02T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 89.38888888888889,
          "Occur Date": "2015-02-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 89.33333333333333,
          "Occur Date": "2015-02-04T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 88.85555555555555,
          "Occur Date": "2015-02-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 88.17777777777778,
          "Occur Date": "2015-02-06T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 88.03333333333333,
          "Occur Date": "2015-02-07T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 87.68888888888888,
          "Occur Date": "2015-02-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 87.64444444444445,
          "Occur Date": "2015-02-09T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 87.26666666666667,
          "Occur Date": "2015-02-10T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.07777777777778,
          "Occur Date": "2015-02-11T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 86.88888888888889,
          "Occur Date": "2015-02-12T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 86.37777777777778,
          "Occur Date": "2015-02-13T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 86.25555555555556,
          "Occur Date": "2015-02-14T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 85.85555555555555,
          "Occur Date": "2015-02-15T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 85.25555555555556,
          "Occur Date": "2015-02-16T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 84.87777777777778,
          "Occur Date": "2015-02-17T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 84.31111111111112,
          "Occur Date": "2015-02-18T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 83.83333333333333,
          "Occur Date": "2015-02-19T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 83.46666666666667,
          "Occur Date": "2015-02-20T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 83.21111111111111,
          "Occur Date": "2015-02-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 82.76666666666667,
          "Occur Date": "2015-02-22T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 82.35555555555555,
          "Occur Date": "2015-02-23T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 81.86666666666666,
          "Occur Date": "2015-02-24T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 81.72222222222223,
          "Occur Date": "2015-02-25T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 81.27777777777777,
          "Occur Date": "2015-02-26T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 81.37777777777778,
          "Occur Date": "2015-02-27T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 81.33333333333333,
          "Occur Date": "2015-02-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.23333333333333,
          "Occur Date": "2015-03-01T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 80.84444444444445,
          "Occur Date": "2015-03-02T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 80.5111111111111,
          "Occur Date": "2015-03-03T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 80.63333333333334,
          "Occur Date": "2015-03-04T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 80.32222222222222,
          "Occur Date": "2015-03-05T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 80,
          "Occur Date": "2015-03-06T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 80.07777777777778,
          "Occur Date": "2015-03-07T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 80.04444444444445,
          "Occur Date": "2015-03-08T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.9,
          "Occur Date": "2015-03-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.41111111111111,
          "Occur Date": "2015-03-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 78.93333333333334,
          "Occur Date": "2015-03-11T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 78.18888888888888,
          "Occur Date": "2015-03-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 77.74444444444444,
          "Occur Date": "2015-03-13T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 77.74444444444444,
          "Occur Date": "2015-03-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 77.35555555555555,
          "Occur Date": "2015-03-15T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 76.9,
          "Occur Date": "2015-03-16T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 76.6,
          "Occur Date": "2015-03-17T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 76.28888888888889,
          "Occur Date": "2015-03-18T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 76.02222222222223,
          "Occur Date": "2015-03-19T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 75.73333333333333,
          "Occur Date": "2015-03-20T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 75.4,
          "Occur Date": "2015-03-21T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 75.16666666666667,
          "Occur Date": "2015-03-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.76666666666667,
          "Occur Date": "2015-03-23T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 74.82222222222222,
          "Occur Date": "2015-03-24T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 75.15555555555555,
          "Occur Date": "2015-03-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 74.93333333333334,
          "Occur Date": "2015-03-26T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 74.83333333333333,
          "Occur Date": "2015-03-27T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 74.85555555555555,
          "Occur Date": "2015-03-28T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 74.71111111111111,
          "Occur Date": "2015-03-29T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 74.5,
          "Occur Date": "2015-03-30T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 74,
          "Occur Date": "2015-03-31T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 74.11111111111111,
          "Occur Date": "2015-04-01T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 74,
          "Occur Date": "2015-04-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.96666666666667,
          "Occur Date": "2015-04-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.91111111111111,
          "Occur Date": "2015-04-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2015-04-05T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2015-04-06T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.74444444444444,
          "Occur Date": "2015-04-07T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.95555555555555,
          "Occur Date": "2015-04-08T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.77777777777777,
          "Occur Date": "2015-04-09T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 74,
          "Occur Date": "2015-04-10T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 73.93333333333334,
          "Occur Date": "2015-04-11T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 73.61111111111111,
          "Occur Date": "2015-04-12T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.7,
          "Occur Date": "2015-04-13T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.91111111111111,
          "Occur Date": "2015-04-14T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 73.6,
          "Occur Date": "2015-04-15T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2015-04-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.32222222222222,
          "Occur Date": "2015-04-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.3,
          "Occur Date": "2015-04-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.07777777777778,
          "Occur Date": "2015-04-19T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.2,
          "Occur Date": "2015-04-20T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 73.04444444444445,
          "Occur Date": "2015-04-21T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 72.93333333333334,
          "Occur Date": "2015-04-22T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.94444444444444,
          "Occur Date": "2015-04-23T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.77777777777777,
          "Occur Date": "2015-04-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2015-04-25T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 72.94444444444444,
          "Occur Date": "2015-04-26T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.74444444444444,
          "Occur Date": "2015-04-27T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 72.52222222222223,
          "Occur Date": "2015-04-28T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.34444444444445,
          "Occur Date": "2015-04-29T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.35555555555555,
          "Occur Date": "2015-04-30T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72.45555555555555,
          "Occur Date": "2015-05-01T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 72.44444444444444,
          "Occur Date": "2015-05-02T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 72.43333333333334,
          "Occur Date": "2015-05-03T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.56666666666666,
          "Occur Date": "2015-05-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.61111111111111,
          "Occur Date": "2015-05-05T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2015-05-06T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2015-05-07T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.15555555555555,
          "Occur Date": "2015-05-08T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.36666666666666,
          "Occur Date": "2015-05-09T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2015-05-10T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.52222222222223,
          "Occur Date": "2015-05-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.83333333333333,
          "Occur Date": "2015-05-12T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 74.14444444444445,
          "Occur Date": "2015-05-13T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 74.25555555555556,
          "Occur Date": "2015-05-14T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 74.33333333333333,
          "Occur Date": "2015-05-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 74.85555555555555,
          "Occur Date": "2015-05-16T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 75.24444444444444,
          "Occur Date": "2015-05-17T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 75.73333333333333,
          "Occur Date": "2015-05-18T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 76.31111111111112,
          "Occur Date": "2015-05-19T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 76.76666666666667,
          "Occur Date": "2015-05-20T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 76.8,
          "Occur Date": "2015-05-21T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 77.02222222222223,
          "Occur Date": "2015-05-22T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 77.47777777777777,
          "Occur Date": "2015-05-23T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 77.82222222222222,
          "Occur Date": "2015-05-24T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 78.07777777777778,
          "Occur Date": "2015-05-25T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 78.37777777777778,
          "Occur Date": "2015-05-26T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.83333333333333,
          "Occur Date": "2015-05-27T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 78.88888888888889,
          "Occur Date": "2015-05-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 78.85555555555555,
          "Occur Date": "2015-05-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.06666666666666,
          "Occur Date": "2015-05-30T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 79.12222222222222,
          "Occur Date": "2015-05-31T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.23333333333333,
          "Occur Date": "2015-06-01T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.02222222222223,
          "Occur Date": "2015-06-02T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 78.86666666666666,
          "Occur Date": "2015-06-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.74444444444444,
          "Occur Date": "2015-06-04T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.13333333333334,
          "Occur Date": "2015-06-05T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 79.41111111111111,
          "Occur Date": "2015-06-06T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 79.61111111111111,
          "Occur Date": "2015-06-07T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 80,
          "Occur Date": "2015-06-08T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 80.37777777777778,
          "Occur Date": "2015-06-09T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 80.44444444444444,
          "Occur Date": "2015-06-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 80.57777777777778,
          "Occur Date": "2015-06-11T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 80.76666666666667,
          "Occur Date": "2015-06-12T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 81.02222222222223,
          "Occur Date": "2015-06-13T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 80.95555555555555,
          "Occur Date": "2015-06-14T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 81.12222222222222,
          "Occur Date": "2015-06-15T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 81.5111111111111,
          "Occur Date": "2015-06-16T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 81.5111111111111,
          "Occur Date": "2015-06-17T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 81.65555555555555,
          "Occur Date": "2015-06-18T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 82.0111111111111,
          "Occur Date": "2015-06-19T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 82.46666666666667,
          "Occur Date": "2015-06-20T00:00:00",
          "Volume": 116
         },
         {
          "90-day": 82.74444444444444,
          "Occur Date": "2015-06-21T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 82.57777777777778,
          "Occur Date": "2015-06-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 83,
          "Occur Date": "2015-06-23T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2015-06-24T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 83.43333333333334,
          "Occur Date": "2015-06-25T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 83.85555555555555,
          "Occur Date": "2015-06-26T00:00:00",
          "Volume": 123
         },
         {
          "90-day": 84.3,
          "Occur Date": "2015-06-27T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 84.38888888888889,
          "Occur Date": "2015-06-28T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.67777777777778,
          "Occur Date": "2015-06-29T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 84.57777777777778,
          "Occur Date": "2015-06-30T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.9,
          "Occur Date": "2015-07-01T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 85.35555555555555,
          "Occur Date": "2015-07-02T00:00:00",
          "Volume": 119
         },
         {
          "90-day": 85.45555555555555,
          "Occur Date": "2015-07-03T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.74444444444444,
          "Occur Date": "2015-07-04T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 85.61111111111111,
          "Occur Date": "2015-07-05T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2015-07-06T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2015-07-07T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2015-07-08T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 85.63333333333334,
          "Occur Date": "2015-07-09T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 85.75555555555556,
          "Occur Date": "2015-07-10T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 86.13333333333334,
          "Occur Date": "2015-07-11T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 86.13333333333334,
          "Occur Date": "2015-07-12T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 86.24444444444444,
          "Occur Date": "2015-07-13T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 86.47777777777777,
          "Occur Date": "2015-07-14T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 86.66666666666667,
          "Occur Date": "2015-07-15T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 86.87777777777778,
          "Occur Date": "2015-07-16T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 87.12222222222222,
          "Occur Date": "2015-07-17T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 87.53333333333333,
          "Occur Date": "2015-07-18T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 87.54444444444445,
          "Occur Date": "2015-07-19T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 87.57777777777778,
          "Occur Date": "2015-07-20T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 87.7,
          "Occur Date": "2015-07-21T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.93333333333334,
          "Occur Date": "2015-07-22T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 87.76666666666667,
          "Occur Date": "2015-07-23T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 87.44444444444444,
          "Occur Date": "2015-07-24T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 87.76666666666667,
          "Occur Date": "2015-07-25T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 87.96666666666667,
          "Occur Date": "2015-07-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 88.28888888888889,
          "Occur Date": "2015-07-27T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 88.45555555555555,
          "Occur Date": "2015-07-28T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 88.56666666666666,
          "Occur Date": "2015-07-29T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.32222222222222,
          "Occur Date": "2015-07-30T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.53333333333333,
          "Occur Date": "2015-07-31T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-08-01T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-08-02T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 88.88888888888889,
          "Occur Date": "2015-08-03T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 88.83333333333333,
          "Occur Date": "2015-08-04T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-08-05T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-08-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.73333333333333,
          "Occur Date": "2015-08-07T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.74444444444444,
          "Occur Date": "2015-08-08T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2015-08-09T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 88.66666666666667,
          "Occur Date": "2015-08-10T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 88.4888888888889,
          "Occur Date": "2015-08-11T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.53333333333333,
          "Occur Date": "2015-08-12T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.58888888888889,
          "Occur Date": "2015-08-13T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 88.71111111111111,
          "Occur Date": "2015-08-14T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 88.87777777777778,
          "Occur Date": "2015-08-15T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-08-16T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 88.64444444444445,
          "Occur Date": "2015-08-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 88.65555555555555,
          "Occur Date": "2015-08-18T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 88.53333333333333,
          "Occur Date": "2015-08-19T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 88.35555555555555,
          "Occur Date": "2015-08-20T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 88.22222222222223,
          "Occur Date": "2015-08-21T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.33333333333333,
          "Occur Date": "2015-08-22T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 88.41111111111111,
          "Occur Date": "2015-08-23T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.5,
          "Occur Date": "2015-08-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2015-08-25T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 88.63333333333334,
          "Occur Date": "2015-08-26T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 88.82222222222222,
          "Occur Date": "2015-08-27T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 89.12222222222222,
          "Occur Date": "2015-08-28T00:00:00",
          "Volume": 117
         },
         {
          "90-day": 89.43333333333334,
          "Occur Date": "2015-08-29T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.74444444444444,
          "Occur Date": "2015-08-30T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 90.07777777777778,
          "Occur Date": "2015-08-31T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 90.21111111111111,
          "Occur Date": "2015-09-01T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 90.13333333333334,
          "Occur Date": "2015-09-02T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 89.8,
          "Occur Date": "2015-09-03T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 89.73333333333333,
          "Occur Date": "2015-09-04T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 89.96666666666667,
          "Occur Date": "2015-09-05T00:00:00",
          "Volume": 115
         },
         {
          "90-day": 90,
          "Occur Date": "2015-09-06T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 90.08888888888889,
          "Occur Date": "2015-09-07T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 90.43333333333334,
          "Occur Date": "2015-09-08T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 90.43333333333334,
          "Occur Date": "2015-09-09T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 90.2,
          "Occur Date": "2015-09-10T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 90.24444444444444,
          "Occur Date": "2015-09-11T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 90.47777777777777,
          "Occur Date": "2015-09-12T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 90.5,
          "Occur Date": "2015-09-13T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 90.36666666666666,
          "Occur Date": "2015-09-14T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 90.62222222222222,
          "Occur Date": "2015-09-15T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 90.66666666666667,
          "Occur Date": "2015-09-16T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 90.37777777777778,
          "Occur Date": "2015-09-17T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 90.17777777777778,
          "Occur Date": "2015-09-18T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 90.05555555555556,
          "Occur Date": "2015-09-19T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 90.14444444444445,
          "Occur Date": "2015-09-20T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 89.77777777777777,
          "Occur Date": "2015-09-21T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 89.86666666666666,
          "Occur Date": "2015-09-22T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 89.55555555555556,
          "Occur Date": "2015-09-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 89.15555555555555,
          "Occur Date": "2015-09-24T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 88.87777777777778,
          "Occur Date": "2015-09-25T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.92222222222222,
          "Occur Date": "2015-09-26T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 88.97777777777777,
          "Occur Date": "2015-09-27T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 89.0111111111111,
          "Occur Date": "2015-09-28T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 88.8,
          "Occur Date": "2015-09-29T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 88.43333333333334,
          "Occur Date": "2015-09-30T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.42222222222222,
          "Occur Date": "2015-10-01T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 88.61111111111111,
          "Occur Date": "2015-10-02T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 88.81111111111112,
          "Occur Date": "2015-10-03T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2015-10-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-10-05T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 88.7,
          "Occur Date": "2015-10-06T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 88.93333333333334,
          "Occur Date": "2015-10-07T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 88.9888888888889,
          "Occur Date": "2015-10-08T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 89.06666666666666,
          "Occur Date": "2015-10-09T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 89.61111111111111,
          "Occur Date": "2015-10-10T00:00:00",
          "Volume": 121
         },
         {
          "90-day": 89.47777777777777,
          "Occur Date": "2015-10-11T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 89.23333333333333,
          "Occur Date": "2015-10-12T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.97777777777777,
          "Occur Date": "2015-10-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-10-14T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 88.45555555555555,
          "Occur Date": "2015-10-15T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 88.0111111111111,
          "Occur Date": "2015-10-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 88.02222222222223,
          "Occur Date": "2015-10-17T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.17777777777778,
          "Occur Date": "2015-10-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 88.26666666666667,
          "Occur Date": "2015-10-19T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 88.16666666666667,
          "Occur Date": "2015-10-20T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 88.46666666666667,
          "Occur Date": "2015-10-21T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 88.61111111111111,
          "Occur Date": "2015-10-22T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 88.76666666666667,
          "Occur Date": "2015-10-23T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 89.25555555555556,
          "Occur Date": "2015-10-24T00:00:00",
          "Volume": 120
         },
         {
          "90-day": 89.05555555555556,
          "Occur Date": "2015-10-25T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 89.18888888888888,
          "Occur Date": "2015-10-26T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 89.25555555555556,
          "Occur Date": "2015-10-27T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 89.24444444444444,
          "Occur Date": "2015-10-28T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 89,
          "Occur Date": "2015-10-29T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 88.73333333333333,
          "Occur Date": "2015-10-30T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 88.94444444444444,
          "Occur Date": "2015-10-31T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 88.75555555555556,
          "Occur Date": "2015-11-01T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 88.5,
          "Occur Date": "2015-11-02T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2015-11-03T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 88.96666666666667,
          "Occur Date": "2015-11-04T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 88.82222222222222,
          "Occur Date": "2015-11-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 88.8,
          "Occur Date": "2015-11-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 89.04444444444445,
          "Occur Date": "2015-11-07T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 88.67777777777778,
          "Occur Date": "2015-11-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 88.77777777777777,
          "Occur Date": "2015-11-09T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 88.66666666666667,
          "Occur Date": "2015-11-10T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 88.45555555555555,
          "Occur Date": "2015-11-11T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 88.16666666666667,
          "Occur Date": "2015-11-12T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 88.12222222222222,
          "Occur Date": "2015-11-13T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 87.87777777777778,
          "Occur Date": "2015-11-14T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 87.64444444444445,
          "Occur Date": "2015-11-15T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 87.54444444444445,
          "Occur Date": "2015-11-16T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.77777777777777,
          "Occur Date": "2015-11-17T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 87.93333333333334,
          "Occur Date": "2015-11-18T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 87.47777777777777,
          "Occur Date": "2015-11-19T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 87.26666666666667,
          "Occur Date": "2015-11-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 87.03333333333333,
          "Occur Date": "2015-11-21T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 86.92222222222222,
          "Occur Date": "2015-11-22T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 86.63333333333334,
          "Occur Date": "2015-11-23T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 86.42222222222222,
          "Occur Date": "2015-11-24T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 86.33333333333333,
          "Occur Date": "2015-11-25T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 85.63333333333334,
          "Occur Date": "2015-11-26T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 85.65555555555555,
          "Occur Date": "2015-11-27T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 85.56666666666666,
          "Occur Date": "2015-11-28T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 85.21111111111111,
          "Occur Date": "2015-11-29T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 85.17777777777778,
          "Occur Date": "2015-11-30T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 85.32222222222222,
          "Occur Date": "2015-12-01T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 85.41111111111111,
          "Occur Date": "2015-12-02T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 85.18888888888888,
          "Occur Date": "2015-12-03T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 84.93333333333334,
          "Occur Date": "2015-12-04T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 84.72222222222223,
          "Occur Date": "2015-12-05T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 84.4888888888889,
          "Occur Date": "2015-12-06T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 84.11111111111111,
          "Occur Date": "2015-12-07T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 83.95555555555555,
          "Occur Date": "2015-12-08T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 83.9,
          "Occur Date": "2015-12-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 83.75555555555556,
          "Occur Date": "2015-12-10T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 83.66666666666667,
          "Occur Date": "2015-12-11T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 83.6,
          "Occur Date": "2015-12-12T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2015-12-13T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 83.57777777777778,
          "Occur Date": "2015-12-14T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 83.5111111111111,
          "Occur Date": "2015-12-15T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 83.7,
          "Occur Date": "2015-12-16T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 83.43333333333334,
          "Occur Date": "2015-12-17T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 83.38888888888889,
          "Occur Date": "2015-12-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 83.33333333333333,
          "Occur Date": "2015-12-19T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 83.33333333333333,
          "Occur Date": "2015-12-20T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2015-12-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 83.21111111111111,
          "Occur Date": "2015-12-22T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 83.08888888888889,
          "Occur Date": "2015-12-23T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 82.97777777777777,
          "Occur Date": "2015-12-24T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 82.5,
          "Occur Date": "2015-12-25T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 82.18888888888888,
          "Occur Date": "2015-12-26T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 81.85555555555555,
          "Occur Date": "2015-12-27T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 81.55555555555556,
          "Occur Date": "2015-12-28T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 81.41111111111111,
          "Occur Date": "2015-12-29T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 81.37777777777778,
          "Occur Date": "2015-12-30T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 81.36666666666666,
          "Occur Date": "2015-12-31T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 81.3,
          "Occur Date": "2016-01-01T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 81.31111111111112,
          "Occur Date": "2016-01-02T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 81.12222222222222,
          "Occur Date": "2016-01-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 81.08888888888889,
          "Occur Date": "2016-01-04T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 80.84444444444445,
          "Occur Date": "2016-01-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 80.61111111111111,
          "Occur Date": "2016-01-06T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 80.3,
          "Occur Date": "2016-01-07T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.77777777777777,
          "Occur Date": "2016-01-08T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 79.62222222222222,
          "Occur Date": "2016-01-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.55555555555556,
          "Occur Date": "2016-01-10T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.47777777777777,
          "Occur Date": "2016-01-11T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 79.4,
          "Occur Date": "2016-01-12T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 79.54444444444445,
          "Occur Date": "2016-01-13T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 79.56666666666666,
          "Occur Date": "2016-01-14T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 79.45555555555555,
          "Occur Date": "2016-01-15T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.58888888888889,
          "Occur Date": "2016-01-16T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 79.5,
          "Occur Date": "2016-01-17T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.46666666666667,
          "Occur Date": "2016-01-18T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 79.11111111111111,
          "Occur Date": "2016-01-19T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 78.8,
          "Occur Date": "2016-01-20T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 78.31111111111112,
          "Occur Date": "2016-01-21T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 77.61111111111111,
          "Occur Date": "2016-01-22T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 77.63333333333334,
          "Occur Date": "2016-01-23T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 77.35555555555555,
          "Occur Date": "2016-01-24T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 77.33333333333333,
          "Occur Date": "2016-01-25T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 77.27777777777777,
          "Occur Date": "2016-01-26T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 77.42222222222222,
          "Occur Date": "2016-01-27T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 77.45555555555555,
          "Occur Date": "2016-01-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 77.37777777777778,
          "Occur Date": "2016-01-29T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 77.54444444444445,
          "Occur Date": "2016-01-30T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 77.73333333333333,
          "Occur Date": "2016-01-31T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 77.73333333333333,
          "Occur Date": "2016-02-01T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 77.22222222222223,
          "Occur Date": "2016-02-02T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 77.2,
          "Occur Date": "2016-02-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 77.0111111111111,
          "Occur Date": "2016-02-04T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 76.72222222222223,
          "Occur Date": "2016-02-05T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 76.91111111111111,
          "Occur Date": "2016-02-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 76.86666666666666,
          "Occur Date": "2016-02-07T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 76.84444444444445,
          "Occur Date": "2016-02-08T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 76.74444444444444,
          "Occur Date": "2016-02-09T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 76.41111111111111,
          "Occur Date": "2016-02-10T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 76,
          "Occur Date": "2016-02-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 76.25555555555556,
          "Occur Date": "2016-02-12T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 76.26666666666667,
          "Occur Date": "2016-02-13T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 76.14444444444445,
          "Occur Date": "2016-02-14T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 75.64444444444445,
          "Occur Date": "2016-02-15T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 75.28888888888889,
          "Occur Date": "2016-02-16T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 75.42222222222222,
          "Occur Date": "2016-02-17T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 75.43333333333334,
          "Occur Date": "2016-02-18T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 75.45555555555555,
          "Occur Date": "2016-02-19T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 75.53333333333333,
          "Occur Date": "2016-02-20T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 75.23333333333333,
          "Occur Date": "2016-02-21T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 75.25555555555556,
          "Occur Date": "2016-02-22T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 75.0111111111111,
          "Occur Date": "2016-02-23T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 75.08888888888889,
          "Occur Date": "2016-02-24T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.96666666666667,
          "Occur Date": "2016-02-25T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 74.63333333333334,
          "Occur Date": "2016-02-26T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.68888888888888,
          "Occur Date": "2016-02-27T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 74.5,
          "Occur Date": "2016-02-28T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 74.1,
          "Occur Date": "2016-02-29T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 73.77777777777777,
          "Occur Date": "2016-03-01T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 73.44444444444444,
          "Occur Date": "2016-03-02T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2016-03-03T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73,
          "Occur Date": "2016-03-04T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 73.05555555555556,
          "Occur Date": "2016-03-05T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 73,
          "Occur Date": "2016-03-06T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.06666666666666,
          "Occur Date": "2016-03-07T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.13333333333334,
          "Occur Date": "2016-03-08T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 72.93333333333334,
          "Occur Date": "2016-03-09T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.7,
          "Occur Date": "2016-03-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.8,
          "Occur Date": "2016-03-11T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.6,
          "Occur Date": "2016-03-12T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 72.37777777777778,
          "Occur Date": "2016-03-13T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.33333333333333,
          "Occur Date": "2016-03-14T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2016-03-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 72.12222222222222,
          "Occur Date": "2016-03-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 71.92222222222222,
          "Occur Date": "2016-03-17T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 71.72222222222223,
          "Occur Date": "2016-03-18T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.7,
          "Occur Date": "2016-03-19T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2016-03-20T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.27777777777777,
          "Occur Date": "2016-03-21T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 71.22222222222223,
          "Occur Date": "2016-03-22T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.07777777777778,
          "Occur Date": "2016-03-23T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 71.22222222222223,
          "Occur Date": "2016-03-24T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 71.42222222222222,
          "Occur Date": "2016-03-25T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 71.5,
          "Occur Date": "2016-03-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.37777777777778,
          "Occur Date": "2016-03-27T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 71.27777777777777,
          "Occur Date": "2016-03-28T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.25555555555556,
          "Occur Date": "2016-03-29T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.03333333333333,
          "Occur Date": "2016-03-30T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 70.84444444444445,
          "Occur Date": "2016-03-31T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.14444444444445,
          "Occur Date": "2016-04-01T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 71.33333333333333,
          "Occur Date": "2016-04-02T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 71.28888888888889,
          "Occur Date": "2016-04-03T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.3,
          "Occur Date": "2016-04-04T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 71.74444444444444,
          "Occur Date": "2016-04-05T00:00:00",
          "Volume": 111
         },
         {
          "90-day": 71.93333333333334,
          "Occur Date": "2016-04-06T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 72.14444444444445,
          "Occur Date": "2016-04-07T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 72.1,
          "Occur Date": "2016-04-08T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 72.18888888888888,
          "Occur Date": "2016-04-09T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 72.23333333333333,
          "Occur Date": "2016-04-10T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.06666666666666,
          "Occur Date": "2016-04-11T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 72,
          "Occur Date": "2016-04-12T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.91111111111111,
          "Occur Date": "2016-04-13T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 71.68888888888888,
          "Occur Date": "2016-04-14T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 71.54444444444445,
          "Occur Date": "2016-04-15T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 71.44444444444444,
          "Occur Date": "2016-04-16T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 71.35555555555555,
          "Occur Date": "2016-04-17T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.34444444444445,
          "Occur Date": "2016-04-18T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 71.44444444444444,
          "Occur Date": "2016-04-19T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.6,
          "Occur Date": "2016-04-20T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.73333333333333,
          "Occur Date": "2016-04-21T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.72222222222223,
          "Occur Date": "2016-04-22T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.84444444444445,
          "Occur Date": "2016-04-23T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.5111111111111,
          "Occur Date": "2016-04-24T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 71.4888888888889,
          "Occur Date": "2016-04-25T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2016-04-26T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 71.35555555555555,
          "Occur Date": "2016-04-27T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 71.23333333333333,
          "Occur Date": "2016-04-28T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.13333333333334,
          "Occur Date": "2016-04-29T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 70.96666666666667,
          "Occur Date": "2016-04-30T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 70.85555555555555,
          "Occur Date": "2016-05-01T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 70.92222222222222,
          "Occur Date": "2016-05-02T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 70.96666666666667,
          "Occur Date": "2016-05-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.03333333333333,
          "Occur Date": "2016-05-04T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.93333333333334,
          "Occur Date": "2016-05-05T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 70.82222222222222,
          "Occur Date": "2016-05-06T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 70.78888888888889,
          "Occur Date": "2016-05-07T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 70.7,
          "Occur Date": "2016-05-08T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.1,
          "Occur Date": "2016-05-09T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 71.41111111111111,
          "Occur Date": "2016-05-10T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 71.73333333333333,
          "Occur Date": "2016-05-11T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 71.76666666666667,
          "Occur Date": "2016-05-12T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 72.12222222222222,
          "Occur Date": "2016-05-13T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 72.38888888888889,
          "Occur Date": "2016-05-14T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 72.83333333333333,
          "Occur Date": "2016-05-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 73.07777777777778,
          "Occur Date": "2016-05-16T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.25555555555556,
          "Occur Date": "2016-05-17T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.42222222222222,
          "Occur Date": "2016-05-18T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2016-05-19T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2016-05-20T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 74.0111111111111,
          "Occur Date": "2016-05-21T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 74.07777777777778,
          "Occur Date": "2016-05-22T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2016-05-23T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 74.5111111111111,
          "Occur Date": "2016-05-24T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 74.32222222222222,
          "Occur Date": "2016-05-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 74.45555555555555,
          "Occur Date": "2016-05-26T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 74.58888888888889,
          "Occur Date": "2016-05-27T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 74.58888888888889,
          "Occur Date": "2016-05-28T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 74.77777777777777,
          "Occur Date": "2016-05-29T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 75.23333333333333,
          "Occur Date": "2016-05-30T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 75.72222222222223,
          "Occur Date": "2016-05-31T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 75.93333333333334,
          "Occur Date": "2016-06-01T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 76.04444444444445,
          "Occur Date": "2016-06-02T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 75.95555555555555,
          "Occur Date": "2016-06-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 76.26666666666667,
          "Occur Date": "2016-06-04T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 75.96666666666667,
          "Occur Date": "2016-06-05T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 75.93333333333334,
          "Occur Date": "2016-06-06T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 75.96666666666667,
          "Occur Date": "2016-06-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 76.1,
          "Occur Date": "2016-06-08T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 75.9888888888889,
          "Occur Date": "2016-06-09T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 75.86666666666666,
          "Occur Date": "2016-06-10T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 76.16666666666667,
          "Occur Date": "2016-06-11T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 76.31111111111112,
          "Occur Date": "2016-06-12T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 76.58888888888889,
          "Occur Date": "2016-06-13T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 76.5,
          "Occur Date": "2016-06-14T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 76.85555555555555,
          "Occur Date": "2016-06-15T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 77.27777777777777,
          "Occur Date": "2016-06-16T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 77.45555555555555,
          "Occur Date": "2016-06-17T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 77.63333333333334,
          "Occur Date": "2016-06-18T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 78.08888888888889,
          "Occur Date": "2016-06-19T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 78.12222222222222,
          "Occur Date": "2016-06-20T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.4,
          "Occur Date": "2016-06-21T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 78.64444444444445,
          "Occur Date": "2016-06-22T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 78.6,
          "Occur Date": "2016-06-23T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.57777777777778,
          "Occur Date": "2016-06-24T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 78.81111111111112,
          "Occur Date": "2016-06-25T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 78.97777777777777,
          "Occur Date": "2016-06-26T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.07777777777778,
          "Occur Date": "2016-06-27T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 79.21111111111111,
          "Occur Date": "2016-06-28T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.54444444444445,
          "Occur Date": "2016-06-29T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 79.12222222222222,
          "Occur Date": "2016-06-30T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.25555555555556,
          "Occur Date": "2016-07-01T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 79.24444444444444,
          "Occur Date": "2016-07-02T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 79.4,
          "Occur Date": "2016-07-03T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 78.9888888888889,
          "Occur Date": "2016-07-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 78.93333333333334,
          "Occur Date": "2016-07-05T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 78.75555555555556,
          "Occur Date": "2016-07-06T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 78.94444444444444,
          "Occur Date": "2016-07-07T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 78.94444444444444,
          "Occur Date": "2016-07-08T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 79.13333333333334,
          "Occur Date": "2016-07-09T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 79.31111111111112,
          "Occur Date": "2016-07-10T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 79.34444444444445,
          "Occur Date": "2016-07-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.57777777777778,
          "Occur Date": "2016-07-12T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.71111111111111,
          "Occur Date": "2016-07-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.81111111111112,
          "Occur Date": "2016-07-14T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.91111111111111,
          "Occur Date": "2016-07-15T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.94444444444444,
          "Occur Date": "2016-07-16T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 80.03333333333333,
          "Occur Date": "2016-07-17T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 80.24444444444444,
          "Occur Date": "2016-07-18T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 80.37777777777778,
          "Occur Date": "2016-07-19T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 80.5,
          "Occur Date": "2016-07-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 80.62222222222222,
          "Occur Date": "2016-07-21T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 80.71111111111111,
          "Occur Date": "2016-07-22T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 80.96666666666667,
          "Occur Date": "2016-07-23T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.18888888888888,
          "Occur Date": "2016-07-24T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.55555555555556,
          "Occur Date": "2016-07-25T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 81.47777777777777,
          "Occur Date": "2016-07-26T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.56666666666666,
          "Occur Date": "2016-07-27T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 81.61111111111111,
          "Occur Date": "2016-07-28T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.81111111111112,
          "Occur Date": "2016-07-29T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 82.05555555555556,
          "Occur Date": "2016-07-30T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 82.12222222222222,
          "Occur Date": "2016-07-31T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 82.41111111111111,
          "Occur Date": "2016-08-01T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 82.66666666666667,
          "Occur Date": "2016-08-02T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 82.91111111111111,
          "Occur Date": "2016-08-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 83.08888888888889,
          "Occur Date": "2016-08-04T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 82.9888888888889,
          "Occur Date": "2016-08-05T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 82.97777777777777,
          "Occur Date": "2016-08-06T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 82.92222222222222,
          "Occur Date": "2016-08-07T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 83.23333333333333,
          "Occur Date": "2016-08-08T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 83.12222222222222,
          "Occur Date": "2016-08-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 82.95555555555555,
          "Occur Date": "2016-08-10T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 82.62222222222222,
          "Occur Date": "2016-08-11T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 82.52222222222223,
          "Occur Date": "2016-08-12T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 82.62222222222222,
          "Occur Date": "2016-08-13T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 82.7,
          "Occur Date": "2016-08-14T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 82.78888888888889,
          "Occur Date": "2016-08-15T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 82.8,
          "Occur Date": "2016-08-16T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 82.56666666666666,
          "Occur Date": "2016-08-17T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 82.55555555555556,
          "Occur Date": "2016-08-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 82.68888888888888,
          "Occur Date": "2016-08-19T00:00:00",
          "Volume": 109
         },
         {
          "90-day": 82.96666666666667,
          "Occur Date": "2016-08-20T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 82.92222222222222,
          "Occur Date": "2016-08-21T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 83.18888888888888,
          "Occur Date": "2016-08-22T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 83.38888888888889,
          "Occur Date": "2016-08-23T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 83.24444444444444,
          "Occur Date": "2016-08-24T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 83.14444444444445,
          "Occur Date": "2016-08-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 83.46666666666667,
          "Occur Date": "2016-08-26T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 83.93333333333334,
          "Occur Date": "2016-08-27T00:00:00",
          "Volume": 106
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2016-08-28T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 83.6,
          "Occur Date": "2016-08-29T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 83.71111111111111,
          "Occur Date": "2016-08-30T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 83.73333333333333,
          "Occur Date": "2016-08-31T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 83.95555555555555,
          "Occur Date": "2016-09-01T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 83.88888888888889,
          "Occur Date": "2016-09-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 84.4,
          "Occur Date": "2016-09-03T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 84.37777777777778,
          "Occur Date": "2016-09-04T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 84.63333333333334,
          "Occur Date": "2016-09-05T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 84.63333333333334,
          "Occur Date": "2016-09-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 84.74444444444444,
          "Occur Date": "2016-09-07T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 84.81111111111112,
          "Occur Date": "2016-09-08T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 84.76666666666667,
          "Occur Date": "2016-09-09T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 84.87777777777778,
          "Occur Date": "2016-09-10T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 84.92222222222222,
          "Occur Date": "2016-09-11T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 85.23333333333333,
          "Occur Date": "2016-09-12T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.21111111111111,
          "Occur Date": "2016-09-13T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 85.04444444444445,
          "Occur Date": "2016-09-14T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 84.95555555555555,
          "Occur Date": "2016-09-15T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 85.21111111111111,
          "Occur Date": "2016-09-16T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 85.42222222222222,
          "Occur Date": "2016-09-17T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 85.74444444444444,
          "Occur Date": "2016-09-18T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 85.84444444444445,
          "Occur Date": "2016-09-19T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2016-09-20T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 85.95555555555555,
          "Occur Date": "2016-09-21T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 86.14444444444445,
          "Occur Date": "2016-09-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 86.23333333333333,
          "Occur Date": "2016-09-23T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 86.21111111111111,
          "Occur Date": "2016-09-24T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.08888888888889,
          "Occur Date": "2016-09-25T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.05555555555556,
          "Occur Date": "2016-09-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 85.95555555555555,
          "Occur Date": "2016-09-27T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 86.12222222222222,
          "Occur Date": "2016-09-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 85.97777777777777,
          "Occur Date": "2016-09-29T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 86.13333333333334,
          "Occur Date": "2016-09-30T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 86.12222222222222,
          "Occur Date": "2016-10-01T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 86.36666666666666,
          "Occur Date": "2016-10-02T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 86.42222222222222,
          "Occur Date": "2016-10-03T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 86.47777777777777,
          "Occur Date": "2016-10-04T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 86.65555555555555,
          "Occur Date": "2016-10-05T00:00:00",
          "Volume": 105
         },
         {
          "90-day": 86.81111111111112,
          "Occur Date": "2016-10-06T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 86.84444444444445,
          "Occur Date": "2016-10-07T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 87.12222222222222,
          "Occur Date": "2016-10-08T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 87.05555555555556,
          "Occur Date": "2016-10-09T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 87.31111111111112,
          "Occur Date": "2016-10-10T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 87.47777777777777,
          "Occur Date": "2016-10-11T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 87.41111111111111,
          "Occur Date": "2016-10-12T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 87.41111111111111,
          "Occur Date": "2016-10-13T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.36666666666666,
          "Occur Date": "2016-10-14T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 87.71111111111111,
          "Occur Date": "2016-10-15T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 87.63333333333334,
          "Occur Date": "2016-10-16T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 87.72222222222223,
          "Occur Date": "2016-10-17T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 87.75555555555556,
          "Occur Date": "2016-10-18T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 87.61111111111111,
          "Occur Date": "2016-10-19T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 87.62222222222222,
          "Occur Date": "2016-10-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 87.78888888888889,
          "Occur Date": "2016-10-21T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 87.62222222222222,
          "Occur Date": "2016-10-22T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 87.26666666666667,
          "Occur Date": "2016-10-23T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 87.24444444444444,
          "Occur Date": "2016-10-24T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 87.05555555555556,
          "Occur Date": "2016-10-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 86.96666666666667,
          "Occur Date": "2016-10-26T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 86.84444444444445,
          "Occur Date": "2016-10-27T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 86.63333333333334,
          "Occur Date": "2016-10-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 86.86666666666666,
          "Occur Date": "2016-10-29T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 86.56666666666666,
          "Occur Date": "2016-10-30T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 86.35555555555555,
          "Occur Date": "2016-10-31T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.36666666666666,
          "Occur Date": "2016-11-01T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 86.37777777777778,
          "Occur Date": "2016-11-02T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 86.37777777777778,
          "Occur Date": "2016-11-03T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 86.45555555555555,
          "Occur Date": "2016-11-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 86.54444444444445,
          "Occur Date": "2016-11-05T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 86.14444444444445,
          "Occur Date": "2016-11-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 85.9,
          "Occur Date": "2016-11-07T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 85.77777777777777,
          "Occur Date": "2016-11-08T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 85.83333333333333,
          "Occur Date": "2016-11-09T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 85.55555555555556,
          "Occur Date": "2016-11-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 85.45555555555555,
          "Occur Date": "2016-11-11T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 85.2,
          "Occur Date": "2016-11-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 85.08888888888889,
          "Occur Date": "2016-11-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 84.8,
          "Occur Date": "2016-11-14T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 84.84444444444445,
          "Occur Date": "2016-11-15T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 84.71111111111111,
          "Occur Date": "2016-11-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 84.17777777777778,
          "Occur Date": "2016-11-17T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 84.06666666666666,
          "Occur Date": "2016-11-18T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 84.06666666666666,
          "Occur Date": "2016-11-19T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 83.87777777777778,
          "Occur Date": "2016-11-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 83.57777777777778,
          "Occur Date": "2016-11-21T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 83.58888888888889,
          "Occur Date": "2016-11-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 83.57777777777778,
          "Occur Date": "2016-11-23T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 83.05555555555556,
          "Occur Date": "2016-11-24T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 82.76666666666667,
          "Occur Date": "2016-11-25T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 82.87777777777778,
          "Occur Date": "2016-11-26T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 82.54444444444445,
          "Occur Date": "2016-11-27T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 82.43333333333334,
          "Occur Date": "2016-11-28T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 82.47777777777777,
          "Occur Date": "2016-11-29T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 82.23333333333333,
          "Occur Date": "2016-11-30T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 82.25555555555556,
          "Occur Date": "2016-12-01T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 82.21111111111111,
          "Occur Date": "2016-12-02T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 82.24444444444444,
          "Occur Date": "2016-12-03T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 81.9888888888889,
          "Occur Date": "2016-12-04T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 81.86666666666666,
          "Occur Date": "2016-12-05T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 81.88888888888889,
          "Occur Date": "2016-12-06T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 82.06666666666666,
          "Occur Date": "2016-12-07T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 82.0111111111111,
          "Occur Date": "2016-12-08T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 81.78888888888889,
          "Occur Date": "2016-12-09T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 81.67777777777778,
          "Occur Date": "2016-12-10T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 81.37777777777778,
          "Occur Date": "2016-12-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 81.33333333333333,
          "Occur Date": "2016-12-12T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 81.26666666666667,
          "Occur Date": "2016-12-13T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 81.21111111111111,
          "Occur Date": "2016-12-14T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 81.08888888888889,
          "Occur Date": "2016-12-15T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 80.95555555555555,
          "Occur Date": "2016-12-16T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 80.65555555555555,
          "Occur Date": "2016-12-17T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 80.44444444444444,
          "Occur Date": "2016-12-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 80.63333333333334,
          "Occur Date": "2016-12-19T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 80.55555555555556,
          "Occur Date": "2016-12-20T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 80.62222222222222,
          "Occur Date": "2016-12-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 80.91111111111111,
          "Occur Date": "2016-12-22T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 81.04444444444445,
          "Occur Date": "2016-12-23T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 81.2,
          "Occur Date": "2016-12-24T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 80.92222222222222,
          "Occur Date": "2016-12-25T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 80.67777777777778,
          "Occur Date": "2016-12-26T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 80.4888888888889,
          "Occur Date": "2016-12-27T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 80.34444444444445,
          "Occur Date": "2016-12-28T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 80.15555555555555,
          "Occur Date": "2016-12-29T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 80.08888888888889,
          "Occur Date": "2016-12-30T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 80.03333333333333,
          "Occur Date": "2016-12-31T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2017-01-01T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.7,
          "Occur Date": "2017-01-02T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 79.31111111111112,
          "Occur Date": "2017-01-03T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 79.04444444444445,
          "Occur Date": "2017-01-04T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 78.91111111111111,
          "Occur Date": "2017-01-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 78.5111111111111,
          "Occur Date": "2017-01-06T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 78.26666666666667,
          "Occur Date": "2017-01-07T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 77.72222222222223,
          "Occur Date": "2017-01-08T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 77.53333333333333,
          "Occur Date": "2017-01-09T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 77.46666666666667,
          "Occur Date": "2017-01-10T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 77.47777777777777,
          "Occur Date": "2017-01-11T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 77.27777777777777,
          "Occur Date": "2017-01-12T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 76.95555555555555,
          "Occur Date": "2017-01-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 76.88888888888889,
          "Occur Date": "2017-01-14T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 76.52222222222223,
          "Occur Date": "2017-01-15T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 76.46666666666667,
          "Occur Date": "2017-01-16T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 76.34444444444445,
          "Occur Date": "2017-01-17T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 76.15555555555555,
          "Occur Date": "2017-01-18T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 75.7,
          "Occur Date": "2017-01-19T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 75.56666666666666,
          "Occur Date": "2017-01-20T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 75.7,
          "Occur Date": "2017-01-21T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 75.66666666666667,
          "Occur Date": "2017-01-22T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 75.56666666666666,
          "Occur Date": "2017-01-23T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 75.43333333333334,
          "Occur Date": "2017-01-24T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 75.34444444444445,
          "Occur Date": "2017-01-25T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 75.06666666666666,
          "Occur Date": "2017-01-26T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.88888888888889,
          "Occur Date": "2017-01-27T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 74.96666666666667,
          "Occur Date": "2017-01-28T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 74.86666666666666,
          "Occur Date": "2017-01-29T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 74.55555555555556,
          "Occur Date": "2017-01-30T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.26666666666667,
          "Occur Date": "2017-01-31T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 74.11111111111111,
          "Occur Date": "2017-02-01T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 73.96666666666667,
          "Occur Date": "2017-02-02T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 73.82222222222222,
          "Occur Date": "2017-02-03T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.66666666666667,
          "Occur Date": "2017-02-04T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 73.75555555555556,
          "Occur Date": "2017-02-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 73.8,
          "Occur Date": "2017-02-06T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.68888888888888,
          "Occur Date": "2017-02-07T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73.5,
          "Occur Date": "2017-02-08T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 73.26666666666667,
          "Occur Date": "2017-02-09T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.3,
          "Occur Date": "2017-02-10T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 73.12222222222222,
          "Occur Date": "2017-02-11T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 73.05555555555556,
          "Occur Date": "2017-02-12T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.23333333333333,
          "Occur Date": "2017-02-13T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2017-02-14T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.34444444444445,
          "Occur Date": "2017-02-15T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.1,
          "Occur Date": "2017-02-16T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.83333333333333,
          "Occur Date": "2017-02-17T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 72.62222222222222,
          "Occur Date": "2017-02-18T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 72.68888888888888,
          "Occur Date": "2017-02-19T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.78888888888889,
          "Occur Date": "2017-02-20T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.6,
          "Occur Date": "2017-02-21T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 72.67777777777778,
          "Occur Date": "2017-02-22T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 72.4,
          "Occur Date": "2017-02-23T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 72.04444444444445,
          "Occur Date": "2017-02-24T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 72.07777777777778,
          "Occur Date": "2017-02-25T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 71.66666666666667,
          "Occur Date": "2017-02-26T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 71.45555555555555,
          "Occur Date": "2017-02-27T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 71.33333333333333,
          "Occur Date": "2017-02-28T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 70.9888888888889,
          "Occur Date": "2017-03-01T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 70.43333333333334,
          "Occur Date": "2017-03-02T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 70.14444444444445,
          "Occur Date": "2017-03-03T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 70.14444444444445,
          "Occur Date": "2017-03-04T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 69.93333333333334,
          "Occur Date": "2017-03-05T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 69.63333333333334,
          "Occur Date": "2017-03-06T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 69.26666666666667,
          "Occur Date": "2017-03-07T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 68.97777777777777,
          "Occur Date": "2017-03-08T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 68.62222222222222,
          "Occur Date": "2017-03-09T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 68.34444444444445,
          "Occur Date": "2017-03-10T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 68.22222222222223,
          "Occur Date": "2017-03-11T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 67.75555555555556,
          "Occur Date": "2017-03-12T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 67.36666666666666,
          "Occur Date": "2017-03-13T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 67.0111111111111,
          "Occur Date": "2017-03-14T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 66.5,
          "Occur Date": "2017-03-15T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 65.86666666666666,
          "Occur Date": "2017-03-16T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 65.74444444444444,
          "Occur Date": "2017-03-17T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 65.34444444444445,
          "Occur Date": "2017-03-18T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 64.78888888888889,
          "Occur Date": "2017-03-19T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 64.37777777777778,
          "Occur Date": "2017-03-20T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 63.93333333333333,
          "Occur Date": "2017-03-21T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 63.4,
          "Occur Date": "2017-03-22T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 62.977777777777774,
          "Occur Date": "2017-03-23T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 62.74444444444445,
          "Occur Date": "2017-03-24T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 62.94444444444444,
          "Occur Date": "2017-03-25T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 62.855555555555554,
          "Occur Date": "2017-03-26T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 62.65555555555556,
          "Occur Date": "2017-03-27T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 62.5,
          "Occur Date": "2017-03-28T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 62.4,
          "Occur Date": "2017-03-29T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 62.233333333333334,
          "Occur Date": "2017-03-30T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 61.81111111111111,
          "Occur Date": "2017-03-31T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 61.62222222222222,
          "Occur Date": "2017-04-01T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 61.577777777777776,
          "Occur Date": "2017-04-02T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.5,
          "Occur Date": "2017-04-03T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 61.37777777777778,
          "Occur Date": "2017-04-04T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 61.1,
          "Occur Date": "2017-04-05T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 61.22222222222222,
          "Occur Date": "2017-04-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 61.333333333333336,
          "Occur Date": "2017-04-07T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 61.48888888888889,
          "Occur Date": "2017-04-08T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 61.388888888888886,
          "Occur Date": "2017-04-09T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 61.25555555555555,
          "Occur Date": "2017-04-10T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 61.06666666666667,
          "Occur Date": "2017-04-11T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 61.34444444444444,
          "Occur Date": "2017-04-12T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 61.34444444444444,
          "Occur Date": "2017-04-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 61.233333333333334,
          "Occur Date": "2017-04-14T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.34444444444444,
          "Occur Date": "2017-04-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 61.144444444444446,
          "Occur Date": "2017-04-16T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 61.12222222222222,
          "Occur Date": "2017-04-17T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.111111111111114,
          "Occur Date": "2017-04-18T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 61.25555555555555,
          "Occur Date": "2017-04-19T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 61.522222222222226,
          "Occur Date": "2017-04-20T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 61.4,
          "Occur Date": "2017-04-21T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 61.36666666666667,
          "Occur Date": "2017-04-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 61.4,
          "Occur Date": "2017-04-23T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.34444444444444,
          "Occur Date": "2017-04-24T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 61.388888888888886,
          "Occur Date": "2017-04-25T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 61.58888888888889,
          "Occur Date": "2017-04-26T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 61.22222222222222,
          "Occur Date": "2017-04-27T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 61.2,
          "Occur Date": "2017-04-28T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 61.388888888888886,
          "Occur Date": "2017-04-29T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 61.43333333333333,
          "Occur Date": "2017-04-30T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 61.6,
          "Occur Date": "2017-05-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 61.6,
          "Occur Date": "2017-05-02T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 61.577777777777776,
          "Occur Date": "2017-05-03T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 61.6,
          "Occur Date": "2017-05-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 61.87777777777778,
          "Occur Date": "2017-05-05T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 62.111111111111114,
          "Occur Date": "2017-05-06T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 62.144444444444446,
          "Occur Date": "2017-05-07T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 62.25555555555555,
          "Occur Date": "2017-05-08T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 62.53333333333333,
          "Occur Date": "2017-05-09T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 62.455555555555556,
          "Occur Date": "2017-05-10T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 62.41111111111111,
          "Occur Date": "2017-05-11T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 62.48888888888889,
          "Occur Date": "2017-05-12T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 62.611111111111114,
          "Occur Date": "2017-05-13T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 62.355555555555554,
          "Occur Date": "2017-05-14T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 62.477777777777774,
          "Occur Date": "2017-05-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 62.422222222222224,
          "Occur Date": "2017-05-16T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 62.51111111111111,
          "Occur Date": "2017-05-17T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 62.666666666666664,
          "Occur Date": "2017-05-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 62.855555555555554,
          "Occur Date": "2017-05-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 63.03333333333333,
          "Occur Date": "2017-05-20T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 62.955555555555556,
          "Occur Date": "2017-05-21T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 63.144444444444446,
          "Occur Date": "2017-05-22T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 63.355555555555554,
          "Occur Date": "2017-05-23T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 63.53333333333333,
          "Occur Date": "2017-05-24T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 63.855555555555554,
          "Occur Date": "2017-05-25T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 63.888888888888886,
          "Occur Date": "2017-05-26T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 64.18888888888888,
          "Occur Date": "2017-05-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 64.26666666666667,
          "Occur Date": "2017-05-28T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 64.43333333333334,
          "Occur Date": "2017-05-29T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 64.6,
          "Occur Date": "2017-05-30T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 64.85555555555555,
          "Occur Date": "2017-05-31T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 65.15555555555555,
          "Occur Date": "2017-06-01T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 65.13333333333334,
          "Occur Date": "2017-06-02T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 65.4,
          "Occur Date": "2017-06-03T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 65.4888888888889,
          "Occur Date": "2017-06-04T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 65.63333333333334,
          "Occur Date": "2017-06-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 65.78888888888889,
          "Occur Date": "2017-06-06T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 66.1,
          "Occur Date": "2017-06-07T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 66.24444444444444,
          "Occur Date": "2017-06-08T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 66.37777777777778,
          "Occur Date": "2017-06-09T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 66.74444444444444,
          "Occur Date": "2017-06-10T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 67.03333333333333,
          "Occur Date": "2017-06-11T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 67.14444444444445,
          "Occur Date": "2017-06-12T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 67.5,
          "Occur Date": "2017-06-13T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 67.87777777777778,
          "Occur Date": "2017-06-14T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 68,
          "Occur Date": "2017-06-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 68.54444444444445,
          "Occur Date": "2017-06-16T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 68.77777777777777,
          "Occur Date": "2017-06-17T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 69.18888888888888,
          "Occur Date": "2017-06-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 69.46666666666667,
          "Occur Date": "2017-06-19T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 69.65555555555555,
          "Occur Date": "2017-06-20T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.03333333333333,
          "Occur Date": "2017-06-21T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 70.15555555555555,
          "Occur Date": "2017-06-22T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 70.04444444444445,
          "Occur Date": "2017-06-23T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 70.24444444444444,
          "Occur Date": "2017-06-24T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.53333333333333,
          "Occur Date": "2017-06-25T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 70.71111111111111,
          "Occur Date": "2017-06-26T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.02222222222223,
          "Occur Date": "2017-06-27T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.13333333333334,
          "Occur Date": "2017-06-28T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.16666666666667,
          "Occur Date": "2017-06-29T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 71.27777777777777,
          "Occur Date": "2017-06-30T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 71.27777777777777,
          "Occur Date": "2017-07-01T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.33333333333333,
          "Occur Date": "2017-07-02T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2017-07-03T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 71.82222222222222,
          "Occur Date": "2017-07-04T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.86666666666666,
          "Occur Date": "2017-07-05T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.97777777777777,
          "Occur Date": "2017-07-06T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.9,
          "Occur Date": "2017-07-07T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 72.13333333333334,
          "Occur Date": "2017-07-08T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2017-07-09T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 72.35555555555555,
          "Occur Date": "2017-07-10T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 72.4,
          "Occur Date": "2017-07-11T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.42222222222222,
          "Occur Date": "2017-07-12T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.57777777777778,
          "Occur Date": "2017-07-13T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.71111111111111,
          "Occur Date": "2017-07-14T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 72.96666666666667,
          "Occur Date": "2017-07-15T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.03333333333333,
          "Occur Date": "2017-07-16T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.34444444444445,
          "Occur Date": "2017-07-17T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2017-07-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2017-07-19T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2017-07-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2017-07-21T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 73.66666666666667,
          "Occur Date": "2017-07-22T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2017-07-23T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 73.63333333333334,
          "Occur Date": "2017-07-24T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 73.75555555555556,
          "Occur Date": "2017-07-25T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 74.28888888888889,
          "Occur Date": "2017-07-26T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 74.35555555555555,
          "Occur Date": "2017-07-27T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 74.35555555555555,
          "Occur Date": "2017-07-28T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 74.58888888888889,
          "Occur Date": "2017-07-29T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 74.42222222222222,
          "Occur Date": "2017-07-30T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 74.61111111111111,
          "Occur Date": "2017-07-31T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 74.67777777777778,
          "Occur Date": "2017-08-01T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 74.55555555555556,
          "Occur Date": "2017-08-02T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 74.31111111111112,
          "Occur Date": "2017-08-03T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 74.22222222222223,
          "Occur Date": "2017-08-04T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.33333333333333,
          "Occur Date": "2017-08-05T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 74.17777777777778,
          "Occur Date": "2017-08-06T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 74.21111111111111,
          "Occur Date": "2017-08-07T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 74.2,
          "Occur Date": "2017-08-08T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.02222222222223,
          "Occur Date": "2017-08-09T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 74.05555555555556,
          "Occur Date": "2017-08-10T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 74.04444444444445,
          "Occur Date": "2017-08-11T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.25555555555556,
          "Occur Date": "2017-08-12T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 74.24444444444444,
          "Occur Date": "2017-08-13T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 74.36666666666666,
          "Occur Date": "2017-08-14T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2017-08-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 74.3,
          "Occur Date": "2017-08-16T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 74.38888888888889,
          "Occur Date": "2017-08-17T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2017-08-18T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 74.83333333333333,
          "Occur Date": "2017-08-19T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 74.91111111111111,
          "Occur Date": "2017-08-20T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 74.73333333333333,
          "Occur Date": "2017-08-21T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 74.86666666666666,
          "Occur Date": "2017-08-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 74.67777777777778,
          "Occur Date": "2017-08-23T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 74.75555555555556,
          "Occur Date": "2017-08-24T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 74.73333333333333,
          "Occur Date": "2017-08-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 75.02222222222223,
          "Occur Date": "2017-08-26T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 75.02222222222223,
          "Occur Date": "2017-08-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 75.04444444444445,
          "Occur Date": "2017-08-28T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 75.12222222222222,
          "Occur Date": "2017-08-29T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.91111111111111,
          "Occur Date": "2017-08-30T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 74.88888888888889,
          "Occur Date": "2017-08-31T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 74.94444444444444,
          "Occur Date": "2017-09-01T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 75.3,
          "Occur Date": "2017-09-02T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 75.45555555555555,
          "Occur Date": "2017-09-03T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 75.6,
          "Occur Date": "2017-09-04T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 75.7,
          "Occur Date": "2017-09-05T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 75.64444444444445,
          "Occur Date": "2017-09-06T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 75.66666666666667,
          "Occur Date": "2017-09-07T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 75.64444444444445,
          "Occur Date": "2017-09-08T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 75.65555555555555,
          "Occur Date": "2017-09-09T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 75.88888888888889,
          "Occur Date": "2017-09-10T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 75.55555555555556,
          "Occur Date": "2017-09-11T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 75.8,
          "Occur Date": "2017-09-12T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 75.92222222222222,
          "Occur Date": "2017-09-13T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 75.68888888888888,
          "Occur Date": "2017-09-14T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 75.83333333333333,
          "Occur Date": "2017-09-15T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 75.82222222222222,
          "Occur Date": "2017-09-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 75.85555555555555,
          "Occur Date": "2017-09-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 75.9,
          "Occur Date": "2017-09-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 75.85555555555555,
          "Occur Date": "2017-09-19T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 76.0111111111111,
          "Occur Date": "2017-09-20T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 76.4,
          "Occur Date": "2017-09-21T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 76.38888888888889,
          "Occur Date": "2017-09-22T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 76.61111111111111,
          "Occur Date": "2017-09-23T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 76.54444444444445,
          "Occur Date": "2017-09-24T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 76.67777777777778,
          "Occur Date": "2017-09-25T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 76.6,
          "Occur Date": "2017-09-26T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 76.72222222222223,
          "Occur Date": "2017-09-27T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 76.73333333333333,
          "Occur Date": "2017-09-28T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 76.88888888888889,
          "Occur Date": "2017-09-29T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 77.13333333333334,
          "Occur Date": "2017-09-30T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 77.05555555555556,
          "Occur Date": "2017-10-01T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 77.28888888888889,
          "Occur Date": "2017-10-02T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 77.14444444444445,
          "Occur Date": "2017-10-03T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 77.05555555555556,
          "Occur Date": "2017-10-04T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 77.13333333333334,
          "Occur Date": "2017-10-05T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 77.12222222222222,
          "Occur Date": "2017-10-06T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 77.66666666666667,
          "Occur Date": "2017-10-07T00:00:00",
          "Volume": 107
         },
         {
          "90-day": 77.55555555555556,
          "Occur Date": "2017-10-08T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 77.56666666666666,
          "Occur Date": "2017-10-09T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 77.64444444444445,
          "Occur Date": "2017-10-10T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 77.71111111111111,
          "Occur Date": "2017-10-11T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 77.61111111111111,
          "Occur Date": "2017-10-12T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 77.65555555555555,
          "Occur Date": "2017-10-13T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 77.76666666666667,
          "Occur Date": "2017-10-14T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 77.46666666666667,
          "Occur Date": "2017-10-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 77.6,
          "Occur Date": "2017-10-16T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 77.38888888888889,
          "Occur Date": "2017-10-17T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 77.31111111111112,
          "Occur Date": "2017-10-18T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 77.5111111111111,
          "Occur Date": "2017-10-19T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 77.37777777777778,
          "Occur Date": "2017-10-20T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 77.82222222222222,
          "Occur Date": "2017-10-21T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 78,
          "Occur Date": "2017-10-22T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 77.77777777777777,
          "Occur Date": "2017-10-23T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 77.61111111111111,
          "Occur Date": "2017-10-24T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 77.58888888888889,
          "Occur Date": "2017-10-25T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 77.32222222222222,
          "Occur Date": "2017-10-26T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 77.2,
          "Occur Date": "2017-10-27T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 77.61111111111111,
          "Occur Date": "2017-10-28T00:00:00",
          "Volume": 102
         },
         {
          "90-day": 77.58888888888889,
          "Occur Date": "2017-10-29T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 77.67777777777778,
          "Occur Date": "2017-10-30T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 77.96666666666667,
          "Occur Date": "2017-10-31T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 78.34444444444445,
          "Occur Date": "2017-11-01T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 78.28888888888889,
          "Occur Date": "2017-11-02T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 78.31111111111112,
          "Occur Date": "2017-11-03T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 78.4888888888889,
          "Occur Date": "2017-11-04T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 78.53333333333333,
          "Occur Date": "2017-11-05T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 78.9,
          "Occur Date": "2017-11-06T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 79.38888888888889,
          "Occur Date": "2017-11-07T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 79.56666666666666,
          "Occur Date": "2017-11-08T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.58888888888889,
          "Occur Date": "2017-11-09T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.6,
          "Occur Date": "2017-11-10T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 79.83333333333333,
          "Occur Date": "2017-11-11T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 79.73333333333333,
          "Occur Date": "2017-11-12T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 79.57777777777778,
          "Occur Date": "2017-11-13T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 79.64444444444445,
          "Occur Date": "2017-11-14T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.57777777777778,
          "Occur Date": "2017-11-15T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.45555555555555,
          "Occur Date": "2017-11-16T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 79.52222222222223,
          "Occur Date": "2017-11-17T00:00:00",
          "Volume": 110
         },
         {
          "90-day": 79.58888888888889,
          "Occur Date": "2017-11-18T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.85555555555555,
          "Occur Date": "2017-11-19T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.8,
          "Occur Date": "2017-11-20T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 79.73333333333333,
          "Occur Date": "2017-11-21T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2017-11-22T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 79.47777777777777,
          "Occur Date": "2017-11-23T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 79.34444444444445,
          "Occur Date": "2017-11-24T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.41111111111111,
          "Occur Date": "2017-11-25T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.4888888888889,
          "Occur Date": "2017-11-26T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.58888888888889,
          "Occur Date": "2017-11-27T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 79.77777777777777,
          "Occur Date": "2017-11-28T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 80,
          "Occur Date": "2017-11-29T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 80,
          "Occur Date": "2017-11-30T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 79.88888888888889,
          "Occur Date": "2017-12-01T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 79.9,
          "Occur Date": "2017-12-02T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.73333333333333,
          "Occur Date": "2017-12-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 79.61111111111111,
          "Occur Date": "2017-12-04T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 79.5111111111111,
          "Occur Date": "2017-12-05T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 79.43333333333334,
          "Occur Date": "2017-12-06T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 79.67777777777778,
          "Occur Date": "2017-12-07T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 79.74444444444444,
          "Occur Date": "2017-12-08T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 79.52222222222223,
          "Occur Date": "2017-12-09T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 79.87777777777778,
          "Occur Date": "2017-12-10T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 79.68888888888888,
          "Occur Date": "2017-12-11T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.74444444444444,
          "Occur Date": "2017-12-12T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2017-12-13T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.87777777777778,
          "Occur Date": "2017-12-14T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 79.86666666666666,
          "Occur Date": "2017-12-15T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 80.0111111111111,
          "Occur Date": "2017-12-16T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 80.0111111111111,
          "Occur Date": "2017-12-17T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.88888888888889,
          "Occur Date": "2017-12-18T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 79.67777777777778,
          "Occur Date": "2017-12-19T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 79.58888888888889,
          "Occur Date": "2017-12-20T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 79.64444444444445,
          "Occur Date": "2017-12-21T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 79.68888888888888,
          "Occur Date": "2017-12-22T00:00:00",
          "Volume": 101
         },
         {
          "90-day": 79.82222222222222,
          "Occur Date": "2017-12-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 79.54444444444445,
          "Occur Date": "2017-12-24T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 79.21111111111111,
          "Occur Date": "2017-12-25T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 79.21111111111111,
          "Occur Date": "2017-12-26T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 79.35555555555555,
          "Occur Date": "2017-12-27T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 79.33333333333333,
          "Occur Date": "2017-12-28T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 78.92222222222222,
          "Occur Date": "2017-12-29T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 78.84444444444445,
          "Occur Date": "2017-12-30T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 78.8,
          "Occur Date": "2017-12-31T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 78.74444444444444,
          "Occur Date": "2018-01-01T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 78.68888888888888,
          "Occur Date": "2018-01-02T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 78.58888888888889,
          "Occur Date": "2018-01-03T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 78.35555555555555,
          "Occur Date": "2018-01-04T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 77.75555555555556,
          "Occur Date": "2018-01-05T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 77.6,
          "Occur Date": "2018-01-06T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 77.31111111111112,
          "Occur Date": "2018-01-07T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 77.23333333333333,
          "Occur Date": "2018-01-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 77.1,
          "Occur Date": "2018-01-09T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 76.88888888888889,
          "Occur Date": "2018-01-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 76.66666666666667,
          "Occur Date": "2018-01-11T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 76.54444444444445,
          "Occur Date": "2018-01-12T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 76.6,
          "Occur Date": "2018-01-13T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 76.22222222222223,
          "Occur Date": "2018-01-14T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 76.23333333333333,
          "Occur Date": "2018-01-15T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 76.16666666666667,
          "Occur Date": "2018-01-16T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 75.63333333333334,
          "Occur Date": "2018-01-17T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 75.67777777777778,
          "Occur Date": "2018-01-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 75.46666666666667,
          "Occur Date": "2018-01-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 75.5,
          "Occur Date": "2018-01-20T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 75.23333333333333,
          "Occur Date": "2018-01-21T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 75.08888888888889,
          "Occur Date": "2018-01-22T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 75.08888888888889,
          "Occur Date": "2018-01-23T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 75.2,
          "Occur Date": "2018-01-24T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 75.07777777777778,
          "Occur Date": "2018-01-25T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 74.76666666666667,
          "Occur Date": "2018-01-26T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 74.74444444444444,
          "Occur Date": "2018-01-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 74.68888888888888,
          "Occur Date": "2018-01-28T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 74.25555555555556,
          "Occur Date": "2018-01-29T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 73.75555555555556,
          "Occur Date": "2018-01-30T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 73.55555555555556,
          "Occur Date": "2018-01-31T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.35555555555555,
          "Occur Date": "2018-02-01T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.33333333333333,
          "Occur Date": "2018-02-02T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.42222222222222,
          "Occur Date": "2018-02-03T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.04444444444445,
          "Occur Date": "2018-02-04T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 72.8,
          "Occur Date": "2018-02-05T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 72.6,
          "Occur Date": "2018-02-06T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 72.47777777777777,
          "Occur Date": "2018-02-07T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.32222222222222,
          "Occur Date": "2018-02-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.06666666666666,
          "Occur Date": "2018-02-09T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.16666666666667,
          "Occur Date": "2018-02-10T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 72.25555555555556,
          "Occur Date": "2018-02-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.17777777777778,
          "Occur Date": "2018-02-12T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2018-02-13T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 71.91111111111111,
          "Occur Date": "2018-02-14T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 71.61111111111111,
          "Occur Date": "2018-02-15T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 71.56666666666666,
          "Occur Date": "2018-02-16T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.58888888888889,
          "Occur Date": "2018-02-17T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 71.47777777777777,
          "Occur Date": "2018-02-18T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.4888888888889,
          "Occur Date": "2018-02-19T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 71.44444444444444,
          "Occur Date": "2018-02-20T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 71.62222222222222,
          "Occur Date": "2018-02-21T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 71.57777777777778,
          "Occur Date": "2018-02-22T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 71.38888888888889,
          "Occur Date": "2018-02-23T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 71.43333333333334,
          "Occur Date": "2018-02-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 71.08888888888889,
          "Occur Date": "2018-02-25T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 70.96666666666667,
          "Occur Date": "2018-02-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 70.65555555555555,
          "Occur Date": "2018-02-27T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 70.27777777777777,
          "Occur Date": "2018-02-28T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 69.96666666666667,
          "Occur Date": "2018-03-01T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 69.67777777777778,
          "Occur Date": "2018-03-02T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 69.7,
          "Occur Date": "2018-03-03T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 69.45555555555555,
          "Occur Date": "2018-03-04T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 69.41111111111111,
          "Occur Date": "2018-03-05T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 69.28888888888889,
          "Occur Date": "2018-03-06T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 68.96666666666667,
          "Occur Date": "2018-03-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 68.7,
          "Occur Date": "2018-03-08T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 68.68888888888888,
          "Occur Date": "2018-03-09T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 68.42222222222222,
          "Occur Date": "2018-03-10T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 68.1,
          "Occur Date": "2018-03-11T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 67.91111111111111,
          "Occur Date": "2018-03-12T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 67.92222222222222,
          "Occur Date": "2018-03-13T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 67.44444444444444,
          "Occur Date": "2018-03-14T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 67.34444444444445,
          "Occur Date": "2018-03-15T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 66.8,
          "Occur Date": "2018-03-16T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 66.65555555555555,
          "Occur Date": "2018-03-17T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 66.46666666666667,
          "Occur Date": "2018-03-18T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 66.26666666666667,
          "Occur Date": "2018-03-19T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 65.74444444444444,
          "Occur Date": "2018-03-20T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 65.42222222222222,
          "Occur Date": "2018-03-21T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 65.11111111111111,
          "Occur Date": "2018-03-22T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 64.93333333333334,
          "Occur Date": "2018-03-23T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.88888888888889,
          "Occur Date": "2018-03-24T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 64.9888888888889,
          "Occur Date": "2018-03-25T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 65,
          "Occur Date": "2018-03-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 64.61111111111111,
          "Occur Date": "2018-03-27T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 64.38888888888889,
          "Occur Date": "2018-03-28T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 64.73333333333333,
          "Occur Date": "2018-03-29T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 64.67777777777778,
          "Occur Date": "2018-03-30T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 64.58888888888889,
          "Occur Date": "2018-03-31T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 64.64444444444445,
          "Occur Date": "2018-04-01T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 64.5111111111111,
          "Occur Date": "2018-04-02T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 64.65555555555555,
          "Occur Date": "2018-04-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 64.71111111111111,
          "Occur Date": "2018-04-04T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.94444444444444,
          "Occur Date": "2018-04-05T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 65.24444444444444,
          "Occur Date": "2018-04-06T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 65.46666666666667,
          "Occur Date": "2018-04-07T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 65.13333333333334,
          "Occur Date": "2018-04-08T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 65.07777777777778,
          "Occur Date": "2018-04-09T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 65.06666666666666,
          "Occur Date": "2018-04-10T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 64.95555555555555,
          "Occur Date": "2018-04-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 64.77777777777777,
          "Occur Date": "2018-04-12T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 64.86666666666666,
          "Occur Date": "2018-04-13T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 64.95555555555555,
          "Occur Date": "2018-04-14T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 64.9,
          "Occur Date": "2018-04-15T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 65.0111111111111,
          "Occur Date": "2018-04-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 65.21111111111111,
          "Occur Date": "2018-04-17T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 65.2,
          "Occur Date": "2018-04-18T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 65.03333333333333,
          "Occur Date": "2018-04-19T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 64.63333333333334,
          "Occur Date": "2018-04-20T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 64.83333333333333,
          "Occur Date": "2018-04-21T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.67777777777778,
          "Occur Date": "2018-04-22T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 64.66666666666667,
          "Occur Date": "2018-04-23T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 64.64444444444445,
          "Occur Date": "2018-04-24T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 64.74444444444444,
          "Occur Date": "2018-04-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 64.53333333333333,
          "Occur Date": "2018-04-26T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 64.43333333333334,
          "Occur Date": "2018-04-27T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 64.56666666666666,
          "Occur Date": "2018-04-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 64.52222222222223,
          "Occur Date": "2018-04-29T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 64.74444444444444,
          "Occur Date": "2018-04-30T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 65.02222222222223,
          "Occur Date": "2018-05-01T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 65.05555555555556,
          "Occur Date": "2018-05-02T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 64.96666666666667,
          "Occur Date": "2018-05-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 64.88888888888889,
          "Occur Date": "2018-05-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 65.2,
          "Occur Date": "2018-05-05T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 65.2,
          "Occur Date": "2018-05-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 65.35555555555555,
          "Occur Date": "2018-05-07T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 65.27777777777777,
          "Occur Date": "2018-05-08T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 65.21111111111111,
          "Occur Date": "2018-05-09T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 65.2,
          "Occur Date": "2018-05-10T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 65.02222222222223,
          "Occur Date": "2018-05-11T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 65,
          "Occur Date": "2018-05-12T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 65.05555555555556,
          "Occur Date": "2018-05-13T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 65.05555555555556,
          "Occur Date": "2018-05-14T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 65.17777777777778,
          "Occur Date": "2018-05-15T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 65.06666666666666,
          "Occur Date": "2018-05-16T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 65.03333333333333,
          "Occur Date": "2018-05-17T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 64.9888888888889,
          "Occur Date": "2018-05-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 65.18888888888888,
          "Occur Date": "2018-05-19T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 65.35555555555555,
          "Occur Date": "2018-05-20T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 65.33333333333333,
          "Occur Date": "2018-05-21T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 65.38888888888889,
          "Occur Date": "2018-05-22T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 65.34444444444445,
          "Occur Date": "2018-05-23T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 65.34444444444445,
          "Occur Date": "2018-05-24T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 65.26666666666667,
          "Occur Date": "2018-05-25T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 65.43333333333334,
          "Occur Date": "2018-05-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 65.62222222222222,
          "Occur Date": "2018-05-27T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 65.83333333333333,
          "Occur Date": "2018-05-28T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 65.9888888888889,
          "Occur Date": "2018-05-29T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 66.32222222222222,
          "Occur Date": "2018-05-30T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 66.5111111111111,
          "Occur Date": "2018-05-31T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 66.5111111111111,
          "Occur Date": "2018-06-01T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 66.82222222222222,
          "Occur Date": "2018-06-02T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 66.9888888888889,
          "Occur Date": "2018-06-03T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 67.03333333333333,
          "Occur Date": "2018-06-04T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 67,
          "Occur Date": "2018-06-05T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 67.22222222222223,
          "Occur Date": "2018-06-06T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 67.34444444444445,
          "Occur Date": "2018-06-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 67.58888888888889,
          "Occur Date": "2018-06-08T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 67.9888888888889,
          "Occur Date": "2018-06-09T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 67.97777777777777,
          "Occur Date": "2018-06-10T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 67.95555555555555,
          "Occur Date": "2018-06-11T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 68.21111111111111,
          "Occur Date": "2018-06-12T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 68.16666666666667,
          "Occur Date": "2018-06-13T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 68.57777777777778,
          "Occur Date": "2018-06-14T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 68.65555555555555,
          "Occur Date": "2018-06-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 69.02222222222223,
          "Occur Date": "2018-06-16T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 69.08888888888889,
          "Occur Date": "2018-06-17T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 69.32222222222222,
          "Occur Date": "2018-06-18T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 69.46666666666667,
          "Occur Date": "2018-06-19T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 69.5111111111111,
          "Occur Date": "2018-06-20T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 69.6,
          "Occur Date": "2018-06-21T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 69.66666666666667,
          "Occur Date": "2018-06-22T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 70.03333333333333,
          "Occur Date": "2018-06-23T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 70.12222222222222,
          "Occur Date": "2018-06-24T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.38888888888889,
          "Occur Date": "2018-06-25T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.55555555555556,
          "Occur Date": "2018-06-26T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 70.3,
          "Occur Date": "2018-06-27T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 70.47777777777777,
          "Occur Date": "2018-06-28T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 70.44444444444444,
          "Occur Date": "2018-06-29T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 70.63333333333334,
          "Occur Date": "2018-06-30T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 71,
          "Occur Date": "2018-07-01T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 70.9888888888889,
          "Occur Date": "2018-07-02T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2018-07-03T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 71.17777777777778,
          "Occur Date": "2018-07-04T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 70.94444444444444,
          "Occur Date": "2018-07-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.28888888888889,
          "Occur Date": "2018-07-06T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 71.7,
          "Occur Date": "2018-07-07T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 71.71111111111111,
          "Occur Date": "2018-07-08T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 72.11111111111111,
          "Occur Date": "2018-07-09T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 72.25555555555556,
          "Occur Date": "2018-07-10T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.41111111111111,
          "Occur Date": "2018-07-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.4,
          "Occur Date": "2018-07-12T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 72.52222222222223,
          "Occur Date": "2018-07-13T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.8,
          "Occur Date": "2018-07-14T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 72.86666666666666,
          "Occur Date": "2018-07-15T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.12222222222222,
          "Occur Date": "2018-07-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.15555555555555,
          "Occur Date": "2018-07-17T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.17777777777778,
          "Occur Date": "2018-07-18T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2018-07-19T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.5,
          "Occur Date": "2018-07-20T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.71111111111111,
          "Occur Date": "2018-07-21T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2018-07-22T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2018-07-23T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.4,
          "Occur Date": "2018-07-24T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.7,
          "Occur Date": "2018-07-25T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 73.74444444444444,
          "Occur Date": "2018-07-26T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.65555555555555,
          "Occur Date": "2018-07-27T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.93333333333334,
          "Occur Date": "2018-07-28T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.87777777777778,
          "Occur Date": "2018-07-29T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 74.04444444444445,
          "Occur Date": "2018-07-30T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 73.92222222222222,
          "Occur Date": "2018-07-31T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 73.9,
          "Occur Date": "2018-08-01T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73.7,
          "Occur Date": "2018-08-02T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 73.52222222222223,
          "Occur Date": "2018-08-03T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2018-08-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2018-08-05T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 73.6,
          "Occur Date": "2018-08-06T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2018-08-07T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.56666666666666,
          "Occur Date": "2018-08-08T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.4888888888889,
          "Occur Date": "2018-08-09T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.61111111111111,
          "Occur Date": "2018-08-10T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.87777777777778,
          "Occur Date": "2018-08-11T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 73.67777777777778,
          "Occur Date": "2018-08-12T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 73.72222222222223,
          "Occur Date": "2018-08-13T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.57777777777778,
          "Occur Date": "2018-08-14T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.4,
          "Occur Date": "2018-08-15T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 73.3,
          "Occur Date": "2018-08-16T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.13333333333334,
          "Occur Date": "2018-08-17T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.25555555555556,
          "Occur Date": "2018-08-18T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 73.37777777777778,
          "Occur Date": "2018-08-19T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.43333333333334,
          "Occur Date": "2018-08-20T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.27777777777777,
          "Occur Date": "2018-08-21T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2018-08-22T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.36666666666666,
          "Occur Date": "2018-08-23T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.42222222222222,
          "Occur Date": "2018-08-24T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.26666666666667,
          "Occur Date": "2018-08-25T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.15555555555555,
          "Occur Date": "2018-08-26T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.1,
          "Occur Date": "2018-08-27T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 72.97777777777777,
          "Occur Date": "2018-08-28T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.9,
          "Occur Date": "2018-08-29T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.75555555555556,
          "Occur Date": "2018-08-30T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 72.62222222222222,
          "Occur Date": "2018-08-31T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.73333333333333,
          "Occur Date": "2018-09-01T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.0111111111111,
          "Occur Date": "2018-09-02T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73,
          "Occur Date": "2018-09-03T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 72.94444444444444,
          "Occur Date": "2018-09-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 72.86666666666666,
          "Occur Date": "2018-09-05T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 72.9,
          "Occur Date": "2018-09-06T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 72.77777777777777,
          "Occur Date": "2018-09-07T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72.76666666666667,
          "Occur Date": "2018-09-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.55555555555556,
          "Occur Date": "2018-09-09T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 72.53333333333333,
          "Occur Date": "2018-09-10T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.64444444444445,
          "Occur Date": "2018-09-11T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.53333333333333,
          "Occur Date": "2018-09-12T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.41111111111111,
          "Occur Date": "2018-09-13T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 72.21111111111111,
          "Occur Date": "2018-09-14T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.46666666666667,
          "Occur Date": "2018-09-15T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 72.53333333333333,
          "Occur Date": "2018-09-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 72.62222222222222,
          "Occur Date": "2018-09-17T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.58888888888889,
          "Occur Date": "2018-09-18T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 72.43333333333334,
          "Occur Date": "2018-09-19T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 72.23333333333333,
          "Occur Date": "2018-09-20T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 72.35555555555555,
          "Occur Date": "2018-09-21T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 72.25555555555556,
          "Occur Date": "2018-09-22T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.16666666666667,
          "Occur Date": "2018-09-23T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.16666666666667,
          "Occur Date": "2018-09-24T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 72.15555555555555,
          "Occur Date": "2018-09-25T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2018-09-26T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72,
          "Occur Date": "2018-09-27T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 71.92222222222222,
          "Occur Date": "2018-09-28T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.92222222222222,
          "Occur Date": "2018-09-29T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.88888888888889,
          "Occur Date": "2018-09-30T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.8,
          "Occur Date": "2018-10-01T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.75555555555556,
          "Occur Date": "2018-10-02T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.77777777777777,
          "Occur Date": "2018-10-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2018-10-04T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2018-10-05T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 71.16666666666667,
          "Occur Date": "2018-10-06T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 70.72222222222223,
          "Occur Date": "2018-10-07T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 70.75555555555556,
          "Occur Date": "2018-10-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.53333333333333,
          "Occur Date": "2018-10-09T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 70.16666666666667,
          "Occur Date": "2018-10-10T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 70,
          "Occur Date": "2018-10-11T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 69.9888888888889,
          "Occur Date": "2018-10-12T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 69.93333333333334,
          "Occur Date": "2018-10-13T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 69.77777777777777,
          "Occur Date": "2018-10-14T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 69.52222222222223,
          "Occur Date": "2018-10-15T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 69.63333333333334,
          "Occur Date": "2018-10-16T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 69.45555555555555,
          "Occur Date": "2018-10-17T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 69.55555555555556,
          "Occur Date": "2018-10-18T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 69.54444444444445,
          "Occur Date": "2018-10-19T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 69.64444444444445,
          "Occur Date": "2018-10-20T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 69.58888888888889,
          "Occur Date": "2018-10-21T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 69.81111111111112,
          "Occur Date": "2018-10-22T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 69.74444444444444,
          "Occur Date": "2018-10-23T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 69.56666666666666,
          "Occur Date": "2018-10-24T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 69.53333333333333,
          "Occur Date": "2018-10-25T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 69.62222222222222,
          "Occur Date": "2018-10-26T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 69.85555555555555,
          "Occur Date": "2018-10-27T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 69.56666666666666,
          "Occur Date": "2018-10-28T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 69.8,
          "Occur Date": "2018-10-29T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 69.95555555555555,
          "Occur Date": "2018-10-30T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 70.2,
          "Occur Date": "2018-10-31T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 70.28888888888889,
          "Occur Date": "2018-11-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 70.22222222222223,
          "Occur Date": "2018-11-02T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 70.12222222222222,
          "Occur Date": "2018-11-03T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 69.97777777777777,
          "Occur Date": "2018-11-04T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 70.15555555555555,
          "Occur Date": "2018-11-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.23333333333333,
          "Occur Date": "2018-11-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.32222222222222,
          "Occur Date": "2018-11-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 70.18888888888888,
          "Occur Date": "2018-11-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 70.02222222222223,
          "Occur Date": "2018-11-09T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 70.3,
          "Occur Date": "2018-11-10T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 70.07777777777778,
          "Occur Date": "2018-11-11T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 70.11111111111111,
          "Occur Date": "2018-11-12T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 70.21111111111111,
          "Occur Date": "2018-11-13T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.37777777777778,
          "Occur Date": "2018-11-14T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 70.21111111111111,
          "Occur Date": "2018-11-15T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 70.21111111111111,
          "Occur Date": "2018-11-16T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 70.03333333333333,
          "Occur Date": "2018-11-17T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 70.1,
          "Occur Date": "2018-11-18T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 70.27777777777777,
          "Occur Date": "2018-11-19T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.35555555555555,
          "Occur Date": "2018-11-20T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 70.4,
          "Occur Date": "2018-11-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 70.07777777777778,
          "Occur Date": "2018-11-22T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 70.14444444444445,
          "Occur Date": "2018-11-23T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 70.23333333333333,
          "Occur Date": "2018-11-24T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 70.3,
          "Occur Date": "2018-11-25T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 70.35555555555555,
          "Occur Date": "2018-11-26T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 70.42222222222222,
          "Occur Date": "2018-11-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.6,
          "Occur Date": "2018-11-28T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 70.66666666666667,
          "Occur Date": "2018-11-29T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.82222222222222,
          "Occur Date": "2018-11-30T00:00:00",
          "Volume": 99
         },
         {
          "90-day": 70.97777777777777,
          "Occur Date": "2018-12-01T00:00:00",
          "Volume": 98
         },
         {
          "90-day": 71.17777777777778,
          "Occur Date": "2018-12-02T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 71.33333333333333,
          "Occur Date": "2018-12-03T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 71.47777777777777,
          "Occur Date": "2018-12-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.14444444444445,
          "Occur Date": "2018-12-05T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 71.11111111111111,
          "Occur Date": "2018-12-06T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.44444444444444,
          "Occur Date": "2018-12-07T00:00:00",
          "Volume": 103
         },
         {
          "90-day": 71.6,
          "Occur Date": "2018-12-08T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.37777777777778,
          "Occur Date": "2018-12-09T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 71.42222222222222,
          "Occur Date": "2018-12-10T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 71.54444444444445,
          "Occur Date": "2018-12-11T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 71.6,
          "Occur Date": "2018-12-12T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72,
          "Occur Date": "2018-12-13T00:00:00",
          "Volume": 104
         },
         {
          "90-day": 72.23333333333333,
          "Occur Date": "2018-12-14T00:00:00",
          "Volume": 108
         },
         {
          "90-day": 72.5,
          "Occur Date": "2018-12-15T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 72.58888888888889,
          "Occur Date": "2018-12-16T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.58888888888889,
          "Occur Date": "2018-12-17T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 72.81111111111112,
          "Occur Date": "2018-12-18T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.18888888888888,
          "Occur Date": "2018-12-19T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 73.17777777777778,
          "Occur Date": "2018-12-20T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 73.5,
          "Occur Date": "2018-12-21T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2018-12-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2018-12-23T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.9,
          "Occur Date": "2018-12-24T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 73.5111111111111,
          "Occur Date": "2018-12-25T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 73.52222222222223,
          "Occur Date": "2018-12-26T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73.4888888888889,
          "Occur Date": "2018-12-27T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 73.37777777777778,
          "Occur Date": "2018-12-28T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2018-12-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.32222222222222,
          "Occur Date": "2018-12-30T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2018-12-31T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.4,
          "Occur Date": "2019-01-01T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.44444444444444,
          "Occur Date": "2019-01-02T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.3,
          "Occur Date": "2019-01-03T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73.52222222222223,
          "Occur Date": "2019-01-04T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.56666666666666,
          "Occur Date": "2019-01-05T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.33333333333333,
          "Occur Date": "2019-01-06T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.63333333333334,
          "Occur Date": "2019-01-07T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.75555555555556,
          "Occur Date": "2019-01-08T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.02222222222223,
          "Occur Date": "2019-01-09T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.74444444444444,
          "Occur Date": "2019-01-10T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 73.81111111111112,
          "Occur Date": "2019-01-11T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.8,
          "Occur Date": "2019-01-12T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.82222222222222,
          "Occur Date": "2019-01-13T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 73.96666666666667,
          "Occur Date": "2019-01-14T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 73.83333333333333,
          "Occur Date": "2019-01-15T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 73.71111111111111,
          "Occur Date": "2019-01-16T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 73.77777777777777,
          "Occur Date": "2019-01-17T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 73.73333333333333,
          "Occur Date": "2019-01-18T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 73.82222222222222,
          "Occur Date": "2019-01-19T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.5111111111111,
          "Occur Date": "2019-01-20T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2019-01-21T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.74444444444444,
          "Occur Date": "2019-01-22T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.58888888888889,
          "Occur Date": "2019-01-23T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 73.28888888888889,
          "Occur Date": "2019-01-24T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.06666666666666,
          "Occur Date": "2019-01-25T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 73.08888888888889,
          "Occur Date": "2019-01-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.63333333333334,
          "Occur Date": "2019-01-27T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 72.35555555555555,
          "Occur Date": "2019-01-28T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 72.11111111111111,
          "Occur Date": "2019-01-29T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 71.92222222222222,
          "Occur Date": "2019-01-30T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 71.78888888888889,
          "Occur Date": "2019-01-31T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 71.88888888888889,
          "Occur Date": "2019-02-01T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.14444444444445,
          "Occur Date": "2019-02-02T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.06666666666666,
          "Occur Date": "2019-02-03T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.91111111111111,
          "Occur Date": "2019-02-04T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 71.91111111111111,
          "Occur Date": "2019-02-05T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.72222222222223,
          "Occur Date": "2019-02-06T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 71.36666666666666,
          "Occur Date": "2019-02-07T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 71.1,
          "Occur Date": "2019-02-08T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.2,
          "Occur Date": "2019-02-09T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 71.06666666666666,
          "Occur Date": "2019-02-10T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 70.93333333333334,
          "Occur Date": "2019-02-11T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 70.66666666666667,
          "Occur Date": "2019-02-12T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 70.72222222222223,
          "Occur Date": "2019-02-13T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 70.34444444444445,
          "Occur Date": "2019-02-14T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 70.42222222222222,
          "Occur Date": "2019-02-15T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 70.28888888888889,
          "Occur Date": "2019-02-16T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 70.02222222222223,
          "Occur Date": "2019-02-17T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 69.7,
          "Occur Date": "2019-02-18T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 69.32222222222222,
          "Occur Date": "2019-02-19T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 69.36666666666666,
          "Occur Date": "2019-02-20T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 69.07777777777778,
          "Occur Date": "2019-02-21T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 69.18888888888888,
          "Occur Date": "2019-02-22T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 69.11111111111111,
          "Occur Date": "2019-02-23T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 68.85555555555555,
          "Occur Date": "2019-02-24T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 68.61111111111111,
          "Occur Date": "2019-02-25T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 68.32222222222222,
          "Occur Date": "2019-02-26T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 68.21111111111111,
          "Occur Date": "2019-02-27T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 67.65555555555555,
          "Occur Date": "2019-02-28T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 67.18888888888888,
          "Occur Date": "2019-03-01T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 66.88888888888889,
          "Occur Date": "2019-03-02T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 66.37777777777778,
          "Occur Date": "2019-03-03T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 66.03333333333333,
          "Occur Date": "2019-03-04T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 65.93333333333334,
          "Occur Date": "2019-03-05T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 65.53333333333333,
          "Occur Date": "2019-03-06T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 64.84444444444445,
          "Occur Date": "2019-03-07T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 64.71111111111111,
          "Occur Date": "2019-03-08T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 64.78888888888889,
          "Occur Date": "2019-03-09T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 64.46666666666667,
          "Occur Date": "2019-03-10T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 64.07777777777778,
          "Occur Date": "2019-03-11T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 63.86666666666667,
          "Occur Date": "2019-03-12T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 63.31111111111111,
          "Occur Date": "2019-03-13T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 62.58888888888889,
          "Occur Date": "2019-03-14T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 62.2,
          "Occur Date": "2019-03-15T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 61.91111111111111,
          "Occur Date": "2019-03-16T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 61.55555555555556,
          "Occur Date": "2019-03-17T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 61.25555555555555,
          "Occur Date": "2019-03-18T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 60.733333333333334,
          "Occur Date": "2019-03-19T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 60.32222222222222,
          "Occur Date": "2019-03-20T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 59.78888888888889,
          "Occur Date": "2019-03-21T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 59.81111111111111,
          "Occur Date": "2019-03-22T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 59.611111111111114,
          "Occur Date": "2019-03-23T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 59.233333333333334,
          "Occur Date": "2019-03-24T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 59.27777777777778,
          "Occur Date": "2019-03-25T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 59.166666666666664,
          "Occur Date": "2019-03-26T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 59.15555555555556,
          "Occur Date": "2019-03-27T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 59.03333333333333,
          "Occur Date": "2019-03-28T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 58.91111111111111,
          "Occur Date": "2019-03-29T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 58.888888888888886,
          "Occur Date": "2019-03-30T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 58.65555555555556,
          "Occur Date": "2019-03-31T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 58.67777777777778,
          "Occur Date": "2019-04-01T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 58.67777777777778,
          "Occur Date": "2019-04-02T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 58.75555555555555,
          "Occur Date": "2019-04-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 58.5,
          "Occur Date": "2019-04-04T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 58.46666666666667,
          "Occur Date": "2019-04-05T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 58.666666666666664,
          "Occur Date": "2019-04-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 58.27777777777778,
          "Occur Date": "2019-04-07T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 58.166666666666664,
          "Occur Date": "2019-04-08T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 57.9,
          "Occur Date": "2019-04-09T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 57.833333333333336,
          "Occur Date": "2019-04-10T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 57.455555555555556,
          "Occur Date": "2019-04-11T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 57.37777777777778,
          "Occur Date": "2019-04-12T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 57.544444444444444,
          "Occur Date": "2019-04-13T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 57.18888888888889,
          "Occur Date": "2019-04-14T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 57.15555555555556,
          "Occur Date": "2019-04-15T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 57.077777777777776,
          "Occur Date": "2019-04-16T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 56.84444444444444,
          "Occur Date": "2019-04-17T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 56.8,
          "Occur Date": "2019-04-18T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 56.84444444444444,
          "Occur Date": "2019-04-19T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 56.91111111111111,
          "Occur Date": "2019-04-20T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 56.666666666666664,
          "Occur Date": "2019-04-21T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 56.53333333333333,
          "Occur Date": "2019-04-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 56.58888888888889,
          "Occur Date": "2019-04-23T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 56.53333333333333,
          "Occur Date": "2019-04-24T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 56.644444444444446,
          "Occur Date": "2019-04-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 56.644444444444446,
          "Occur Date": "2019-04-26T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 57.06666666666667,
          "Occur Date": "2019-04-27T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 57.21111111111111,
          "Occur Date": "2019-04-28T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 57.355555555555554,
          "Occur Date": "2019-04-29T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 57.4,
          "Occur Date": "2019-04-30T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 57.58888888888889,
          "Occur Date": "2019-05-01T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 57.544444444444444,
          "Occur Date": "2019-05-02T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 57.46666666666667,
          "Occur Date": "2019-05-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 57.422222222222224,
          "Occur Date": "2019-05-04T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 57.62222222222222,
          "Occur Date": "2019-05-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 57.78888888888889,
          "Occur Date": "2019-05-06T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 57.84444444444444,
          "Occur Date": "2019-05-07T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 58.12222222222222,
          "Occur Date": "2019-05-08T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 58.31111111111111,
          "Occur Date": "2019-05-09T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 58.522222222222226,
          "Occur Date": "2019-05-10T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 58.78888888888889,
          "Occur Date": "2019-05-11T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 58.93333333333333,
          "Occur Date": "2019-05-12T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 59.31111111111111,
          "Occur Date": "2019-05-13T00:00:00",
          "Volume": 96
         },
         {
          "90-day": 59.55555555555556,
          "Occur Date": "2019-05-14T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 59.77777777777778,
          "Occur Date": "2019-05-15T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 59.71111111111111,
          "Occur Date": "2019-05-16T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 59.922222222222224,
          "Occur Date": "2019-05-17T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 60.144444444444446,
          "Occur Date": "2019-05-18T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 60.41111111111111,
          "Occur Date": "2019-05-19T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 60.71111111111111,
          "Occur Date": "2019-05-20T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 61.17777777777778,
          "Occur Date": "2019-05-21T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 61.43333333333333,
          "Occur Date": "2019-05-22T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 61.2,
          "Occur Date": "2019-05-23T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 61.34444444444444,
          "Occur Date": "2019-05-24T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 61.7,
          "Occur Date": "2019-05-25T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 61.922222222222224,
          "Occur Date": "2019-05-26T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 62.2,
          "Occur Date": "2019-05-27T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 62.37777777777778,
          "Occur Date": "2019-05-28T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 62.7,
          "Occur Date": "2019-05-29T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 62.955555555555556,
          "Occur Date": "2019-05-30T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 63.233333333333334,
          "Occur Date": "2019-05-31T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 63.577777777777776,
          "Occur Date": "2019-06-01T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 63.81111111111111,
          "Occur Date": "2019-06-02T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.1,
          "Occur Date": "2019-06-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 64.37777777777778,
          "Occur Date": "2019-06-04T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 64.72222222222223,
          "Occur Date": "2019-06-05T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 64.84444444444445,
          "Occur Date": "2019-06-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 64.95555555555555,
          "Occur Date": "2019-06-07T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 65.16666666666667,
          "Occur Date": "2019-06-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 65.5111111111111,
          "Occur Date": "2019-06-09T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 65.84444444444445,
          "Occur Date": "2019-06-10T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 65.81111111111112,
          "Occur Date": "2019-06-11T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 66.08888888888889,
          "Occur Date": "2019-06-12T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 66.31111111111112,
          "Occur Date": "2019-06-13T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 66.63333333333334,
          "Occur Date": "2019-06-14T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 66.9888888888889,
          "Occur Date": "2019-06-15T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 67.38888888888889,
          "Occur Date": "2019-06-16T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 67.65555555555555,
          "Occur Date": "2019-06-17T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 67.77777777777777,
          "Occur Date": "2019-06-18T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 68.21111111111111,
          "Occur Date": "2019-06-19T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 68.27777777777777,
          "Occur Date": "2019-06-20T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 68.36666666666666,
          "Occur Date": "2019-06-21T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 68.55555555555556,
          "Occur Date": "2019-06-22T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 68.91111111111111,
          "Occur Date": "2019-06-23T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 69.11111111111111,
          "Occur Date": "2019-06-24T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 69.18888888888888,
          "Occur Date": "2019-06-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 69.53333333333333,
          "Occur Date": "2019-06-26T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 69.58888888888889,
          "Occur Date": "2019-06-27T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 69.47777777777777,
          "Occur Date": "2019-06-28T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 69.54444444444445,
          "Occur Date": "2019-06-29T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 69.4888888888889,
          "Occur Date": "2019-06-30T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 69.73333333333333,
          "Occur Date": "2019-07-01T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 69.54444444444445,
          "Occur Date": "2019-07-02T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 69.67777777777778,
          "Occur Date": "2019-07-03T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 69.71111111111111,
          "Occur Date": "2019-07-04T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 69.8,
          "Occur Date": "2019-07-05T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 70.13333333333334,
          "Occur Date": "2019-07-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.44444444444444,
          "Occur Date": "2019-07-07T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 70.71111111111111,
          "Occur Date": "2019-07-08T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 70.87777777777778,
          "Occur Date": "2019-07-09T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.37777777777778,
          "Occur Date": "2019-07-10T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 71.61111111111111,
          "Occur Date": "2019-07-11T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2019-07-12T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.83333333333333,
          "Occur Date": "2019-07-13T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.96666666666667,
          "Occur Date": "2019-07-14T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.16666666666667,
          "Occur Date": "2019-07-15T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 72.4888888888889,
          "Occur Date": "2019-07-16T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 72.62222222222222,
          "Occur Date": "2019-07-17T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 72.55555555555556,
          "Occur Date": "2019-07-18T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.76666666666667,
          "Occur Date": "2019-07-19T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.94444444444444,
          "Occur Date": "2019-07-20T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.05555555555556,
          "Occur Date": "2019-07-21T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.41111111111111,
          "Occur Date": "2019-07-22T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2019-07-23T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.54444444444445,
          "Occur Date": "2019-07-24T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2019-07-25T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.64444444444445,
          "Occur Date": "2019-07-26T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 73.83333333333333,
          "Occur Date": "2019-07-27T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 73.93333333333334,
          "Occur Date": "2019-07-28T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 74.06666666666666,
          "Occur Date": "2019-07-29T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.95555555555555,
          "Occur Date": "2019-07-30T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 74.13333333333334,
          "Occur Date": "2019-07-31T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 74.17777777777778,
          "Occur Date": "2019-08-01T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 74.3,
          "Occur Date": "2019-08-02T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 74.28888888888889,
          "Occur Date": "2019-08-03T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 74.24444444444444,
          "Occur Date": "2019-08-04T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 74.37777777777778,
          "Occur Date": "2019-08-05T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 74.31111111111112,
          "Occur Date": "2019-08-06T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 74.42222222222222,
          "Occur Date": "2019-08-07T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 74.3,
          "Occur Date": "2019-08-08T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 74.41111111111111,
          "Occur Date": "2019-08-09T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 74.36666666666666,
          "Occur Date": "2019-08-10T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 74.06666666666666,
          "Occur Date": "2019-08-11T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 74.04444444444445,
          "Occur Date": "2019-08-12T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 74.14444444444445,
          "Occur Date": "2019-08-13T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 74.2,
          "Occur Date": "2019-08-14T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 74.14444444444445,
          "Occur Date": "2019-08-15T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 74.22222222222223,
          "Occur Date": "2019-08-16T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 74.2,
          "Occur Date": "2019-08-17T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 74.1,
          "Occur Date": "2019-08-18T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.86666666666666,
          "Occur Date": "2019-08-19T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.7,
          "Occur Date": "2019-08-20T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.67777777777778,
          "Occur Date": "2019-08-21T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 73.56666666666666,
          "Occur Date": "2019-08-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 73.41111111111111,
          "Occur Date": "2019-08-23T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.5111111111111,
          "Occur Date": "2019-08-24T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.41111111111111,
          "Occur Date": "2019-08-25T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 73.28888888888889,
          "Occur Date": "2019-08-26T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2019-08-27T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2019-08-28T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 72.85555555555555,
          "Occur Date": "2019-08-29T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 72.87777777777778,
          "Occur Date": "2019-08-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.04444444444445,
          "Occur Date": "2019-08-31T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.12222222222222,
          "Occur Date": "2019-09-01T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.12222222222222,
          "Occur Date": "2019-09-02T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 72.84444444444445,
          "Occur Date": "2019-09-03T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 72.81111111111112,
          "Occur Date": "2019-09-04T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2019-09-05T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 73.15555555555555,
          "Occur Date": "2019-09-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2019-09-07T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2019-09-08T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 73.2,
          "Occur Date": "2019-09-09T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.02222222222223,
          "Occur Date": "2019-09-10T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.13333333333334,
          "Occur Date": "2019-09-11T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.18888888888888,
          "Occur Date": "2019-09-12T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 73.13333333333334,
          "Occur Date": "2019-09-13T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.88888888888889,
          "Occur Date": "2019-09-14T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2019-09-15T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 72.85555555555555,
          "Occur Date": "2019-09-16T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 72.57777777777778,
          "Occur Date": "2019-09-17T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 72.62222222222222,
          "Occur Date": "2019-09-18T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 72.44444444444444,
          "Occur Date": "2019-09-19T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 72.68888888888888,
          "Occur Date": "2019-09-20T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 72.82222222222222,
          "Occur Date": "2019-09-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 73,
          "Occur Date": "2019-09-22T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 73.04444444444445,
          "Occur Date": "2019-09-23T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 72.86666666666666,
          "Occur Date": "2019-09-24T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72.91111111111111,
          "Occur Date": "2019-09-25T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.21111111111111,
          "Occur Date": "2019-09-26T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.44444444444444,
          "Occur Date": "2019-09-27T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 73.55555555555556,
          "Occur Date": "2019-09-28T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.37777777777778,
          "Occur Date": "2019-09-29T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2019-09-30T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.5,
          "Occur Date": "2019-10-01T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 73.61111111111111,
          "Occur Date": "2019-10-02T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.56666666666666,
          "Occur Date": "2019-10-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 73.4888888888889,
          "Occur Date": "2019-10-04T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 73.45555555555555,
          "Occur Date": "2019-10-05T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 73.38888888888889,
          "Occur Date": "2019-10-06T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.4888888888889,
          "Occur Date": "2019-10-07T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 73.07777777777778,
          "Occur Date": "2019-10-08T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 73.08888888888889,
          "Occur Date": "2019-10-09T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 73.23333333333333,
          "Occur Date": "2019-10-10T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 73.33333333333333,
          "Occur Date": "2019-10-11T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 73.64444444444445,
          "Occur Date": "2019-10-12T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 73.61111111111111,
          "Occur Date": "2019-10-13T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 73.46666666666667,
          "Occur Date": "2019-10-14T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.38888888888889,
          "Occur Date": "2019-10-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.34444444444445,
          "Occur Date": "2019-10-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.22222222222223,
          "Occur Date": "2019-10-17T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 73.31111111111112,
          "Occur Date": "2019-10-18T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.21111111111111,
          "Occur Date": "2019-10-19T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 72.8,
          "Occur Date": "2019-10-20T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 72.85555555555555,
          "Occur Date": "2019-10-21T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.6,
          "Occur Date": "2019-10-22T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 72.58888888888889,
          "Occur Date": "2019-10-23T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 72.31111111111112,
          "Occur Date": "2019-10-24T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72.28888888888889,
          "Occur Date": "2019-10-25T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 72.35555555555555,
          "Occur Date": "2019-10-26T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 72.15555555555555,
          "Occur Date": "2019-10-27T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 72.22222222222223,
          "Occur Date": "2019-10-28T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.05555555555556,
          "Occur Date": "2019-10-29T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.93333333333334,
          "Occur Date": "2019-10-30T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.96666666666667,
          "Occur Date": "2019-10-31T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2019-11-01T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 72.17777777777778,
          "Occur Date": "2019-11-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 72.14444444444445,
          "Occur Date": "2019-11-03T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 72.13333333333334,
          "Occur Date": "2019-11-04T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.04444444444445,
          "Occur Date": "2019-11-05T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.87777777777778,
          "Occur Date": "2019-11-06T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2019-11-07T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 71.61111111111111,
          "Occur Date": "2019-11-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.57777777777778,
          "Occur Date": "2019-11-09T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 71.28888888888889,
          "Occur Date": "2019-11-10T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.14444444444445,
          "Occur Date": "2019-11-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 71.16666666666667,
          "Occur Date": "2019-11-12T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 70.86666666666666,
          "Occur Date": "2019-11-13T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 70.77777777777777,
          "Occur Date": "2019-11-14T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 70.66666666666667,
          "Occur Date": "2019-11-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 70.73333333333333,
          "Occur Date": "2019-11-16T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 70.8,
          "Occur Date": "2019-11-17T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.03333333333333,
          "Occur Date": "2019-11-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 71.13333333333334,
          "Occur Date": "2019-11-19T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.5111111111111,
          "Occur Date": "2019-11-20T00:00:00",
          "Volume": 95
         },
         {
          "90-day": 71.42222222222222,
          "Occur Date": "2019-11-21T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 71.35555555555555,
          "Occur Date": "2019-11-22T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 71.45555555555555,
          "Occur Date": "2019-11-23T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.5,
          "Occur Date": "2019-11-24T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.57777777777778,
          "Occur Date": "2019-11-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2019-11-26T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 71.55555555555556,
          "Occur Date": "2019-11-27T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2019-11-28T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 71.22222222222223,
          "Occur Date": "2019-11-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 71.36666666666666,
          "Occur Date": "2019-11-30T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 71.43333333333334,
          "Occur Date": "2019-12-01T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 71.72222222222223,
          "Occur Date": "2019-12-02T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.8,
          "Occur Date": "2019-12-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.9,
          "Occur Date": "2019-12-04T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 71.78888888888889,
          "Occur Date": "2019-12-05T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.9888888888889,
          "Occur Date": "2019-12-06T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 72.33333333333333,
          "Occur Date": "2019-12-07T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 72.36666666666666,
          "Occur Date": "2019-12-08T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72.77777777777777,
          "Occur Date": "2019-12-09T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 72.7,
          "Occur Date": "2019-12-10T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 72.55555555555556,
          "Occur Date": "2019-12-11T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 72.4888888888889,
          "Occur Date": "2019-12-12T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 72.63333333333334,
          "Occur Date": "2019-12-13T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 72.75555555555556,
          "Occur Date": "2019-12-14T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.14444444444445,
          "Occur Date": "2019-12-15T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 73.35555555555555,
          "Occur Date": "2019-12-16T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 73.32222222222222,
          "Occur Date": "2019-12-17T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 73.37777777777778,
          "Occur Date": "2019-12-18T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.91111111111111,
          "Occur Date": "2019-12-19T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 72.75555555555556,
          "Occur Date": "2019-12-20T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 72.56666666666666,
          "Occur Date": "2019-12-21T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 72.32222222222222,
          "Occur Date": "2019-12-22T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 72.45555555555555,
          "Occur Date": "2019-12-23T00:00:00",
          "Volume": 88
         },
         {
          "90-day": 72.47777777777777,
          "Occur Date": "2019-12-24T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 72.11111111111111,
          "Occur Date": "2019-12-25T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 71.94444444444444,
          "Occur Date": "2019-12-26T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.83333333333333,
          "Occur Date": "2019-12-27T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 71.95555555555555,
          "Occur Date": "2019-12-28T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 72,
          "Occur Date": "2019-12-29T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 72.12222222222222,
          "Occur Date": "2019-12-30T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 72.07777777777778,
          "Occur Date": "2019-12-31T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 72.04444444444445,
          "Occur Date": "2020-01-01T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.85555555555555,
          "Occur Date": "2020-01-02T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 71.91111111111111,
          "Occur Date": "2020-01-03T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 71.84444444444445,
          "Occur Date": "2020-01-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.58888888888889,
          "Occur Date": "2020-01-05T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 71.88888888888889,
          "Occur Date": "2020-01-06T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 71.86666666666666,
          "Occur Date": "2020-01-07T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.63333333333334,
          "Occur Date": "2020-01-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 71.43333333333334,
          "Occur Date": "2020-01-09T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 71.21111111111111,
          "Occur Date": "2020-01-10T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 71.23333333333333,
          "Occur Date": "2020-01-11T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.16666666666667,
          "Occur Date": "2020-01-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 70.96666666666667,
          "Occur Date": "2020-01-13T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 71,
          "Occur Date": "2020-01-14T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 71.08888888888889,
          "Occur Date": "2020-01-15T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.17777777777778,
          "Occur Date": "2020-01-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 71.27777777777777,
          "Occur Date": "2020-01-17T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.6,
          "Occur Date": "2020-01-18T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 71.7,
          "Occur Date": "2020-01-19T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.82222222222222,
          "Occur Date": "2020-01-20T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 72,
          "Occur Date": "2020-01-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.96666666666667,
          "Occur Date": "2020-01-22T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 71.56666666666666,
          "Occur Date": "2020-01-23T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 71.44444444444444,
          "Occur Date": "2020-01-24T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.58888888888889,
          "Occur Date": "2020-01-25T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 71.52222222222223,
          "Occur Date": "2020-01-26T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 71.61111111111111,
          "Occur Date": "2020-01-27T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.57777777777778,
          "Occur Date": "2020-01-28T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.2,
          "Occur Date": "2020-01-29T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 71,
          "Occur Date": "2020-01-30T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 70.9,
          "Occur Date": "2020-01-31T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 71.13333333333334,
          "Occur Date": "2020-02-01T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 71.08888888888889,
          "Occur Date": "2020-02-02T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 70.97777777777777,
          "Occur Date": "2020-02-03T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 71.05555555555556,
          "Occur Date": "2020-02-04T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 70.96666666666667,
          "Occur Date": "2020-02-05T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.06666666666666,
          "Occur Date": "2020-02-06T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 71.12222222222222,
          "Occur Date": "2020-02-07T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 71.06666666666666,
          "Occur Date": "2020-02-08T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 70.95555555555555,
          "Occur Date": "2020-02-09T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 70.83333333333333,
          "Occur Date": "2020-02-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 70.97777777777777,
          "Occur Date": "2020-02-11T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 70.93333333333334,
          "Occur Date": "2020-02-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 70.85555555555555,
          "Occur Date": "2020-02-13T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 70.82222222222222,
          "Occur Date": "2020-02-14T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 70.65555555555555,
          "Occur Date": "2020-02-15T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 70.28888888888889,
          "Occur Date": "2020-02-16T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 70.26666666666667,
          "Occur Date": "2020-02-17T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 69.83333333333333,
          "Occur Date": "2020-02-18T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 70.0111111111111,
          "Occur Date": "2020-02-19T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 69.73333333333333,
          "Occur Date": "2020-02-20T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 69.53333333333333,
          "Occur Date": "2020-02-21T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 69.31111111111112,
          "Occur Date": "2020-02-22T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 69.08888888888889,
          "Occur Date": "2020-02-23T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 69.02222222222223,
          "Occur Date": "2020-02-24T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 68.86666666666666,
          "Occur Date": "2020-02-25T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 68.91111111111111,
          "Occur Date": "2020-02-26T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 68.5,
          "Occur Date": "2020-02-27T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 68.03333333333333,
          "Occur Date": "2020-02-28T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 67.96666666666667,
          "Occur Date": "2020-02-29T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 67.88888888888889,
          "Occur Date": "2020-03-01T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 67.71111111111111,
          "Occur Date": "2020-03-02T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 67.41111111111111,
          "Occur Date": "2020-03-03T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 67.17777777777778,
          "Occur Date": "2020-03-04T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 66.64444444444445,
          "Occur Date": "2020-03-05T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 66.17777777777778,
          "Occur Date": "2020-03-06T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 66.03333333333333,
          "Occur Date": "2020-03-07T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 65.62222222222222,
          "Occur Date": "2020-03-08T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 65.46666666666667,
          "Occur Date": "2020-03-09T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 65.43333333333334,
          "Occur Date": "2020-03-10T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 65.23333333333333,
          "Occur Date": "2020-03-11T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 64.93333333333334,
          "Occur Date": "2020-03-12T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 64.8,
          "Occur Date": "2020-03-13T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 64.44444444444444,
          "Occur Date": "2020-03-14T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 64.25555555555556,
          "Occur Date": "2020-03-15T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.16666666666667,
          "Occur Date": "2020-03-16T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 63.91111111111111,
          "Occur Date": "2020-03-17T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 63.78888888888889,
          "Occur Date": "2020-03-18T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 63.27777777777778,
          "Occur Date": "2020-03-19T00:00:00",
          "Volume": 29
         },
         {
          "90-day": 62.888888888888886,
          "Occur Date": "2020-03-20T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 62.766666666666666,
          "Occur Date": "2020-03-21T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 62.18888888888889,
          "Occur Date": "2020-03-22T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 61.84444444444444,
          "Occur Date": "2020-03-23T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 61.82222222222222,
          "Occur Date": "2020-03-24T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 61.56666666666667,
          "Occur Date": "2020-03-25T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 61.21111111111111,
          "Occur Date": "2020-03-26T00:00:00",
          "Volume": 31
         },
         {
          "90-day": 60.9,
          "Occur Date": "2020-03-27T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 60.544444444444444,
          "Occur Date": "2020-03-28T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 60.22222222222222,
          "Occur Date": "2020-03-29T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 59.977777777777774,
          "Occur Date": "2020-03-30T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 59.611111111111114,
          "Occur Date": "2020-03-31T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 59.67777777777778,
          "Occur Date": "2020-04-01T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 59.111111111111114,
          "Occur Date": "2020-04-02T00:00:00",
          "Volume": 30
         },
         {
          "90-day": 58.766666666666666,
          "Occur Date": "2020-04-03T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 58.37777777777778,
          "Occur Date": "2020-04-04T00:00:00",
          "Volume": 22
         },
         {
          "90-day": 57.84444444444444,
          "Occur Date": "2020-04-05T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 57.355555555555554,
          "Occur Date": "2020-04-06T00:00:00",
          "Volume": 29
         },
         {
          "90-day": 57.044444444444444,
          "Occur Date": "2020-04-07T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 56.77777777777778,
          "Occur Date": "2020-04-08T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 56.41111111111111,
          "Occur Date": "2020-04-09T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 56.01111111111111,
          "Occur Date": "2020-04-10T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 55.72222222222222,
          "Occur Date": "2020-04-11T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 55.6,
          "Occur Date": "2020-04-12T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 55.3,
          "Occur Date": "2020-04-13T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 54.94444444444444,
          "Occur Date": "2020-04-14T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 54.56666666666667,
          "Occur Date": "2020-04-15T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 54.08888888888889,
          "Occur Date": "2020-04-16T00:00:00",
          "Volume": 28
         },
         {
          "90-day": 53.56666666666667,
          "Occur Date": "2020-04-17T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 53.166666666666664,
          "Occur Date": "2020-04-18T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 52.65555555555556,
          "Occur Date": "2020-04-19T00:00:00",
          "Volume": 22
         },
         {
          "90-day": 52.18888888888889,
          "Occur Date": "2020-04-20T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 51.766666666666666,
          "Occur Date": "2020-04-21T00:00:00",
          "Volume": 27
         },
         {
          "90-day": 51.644444444444446,
          "Occur Date": "2020-04-22T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 51.22222222222222,
          "Occur Date": "2020-04-23T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 50.9,
          "Occur Date": "2020-04-24T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 50.78888888888889,
          "Occur Date": "2020-04-25T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 50.28888888888889,
          "Occur Date": "2020-04-26T00:00:00",
          "Volume": 30
         },
         {
          "90-day": 49.94444444444444,
          "Occur Date": "2020-04-27T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 49.833333333333336,
          "Occur Date": "2020-04-28T00:00:00",
          "Volume": 35
         },
         {
          "90-day": 49.37777777777778,
          "Occur Date": "2020-04-29T00:00:00",
          "Volume": 27
         },
         {
          "90-day": 48.86666666666667,
          "Occur Date": "2020-04-30T00:00:00",
          "Volume": 32
         },
         {
          "90-day": 48.32222222222222,
          "Occur Date": "2020-05-01T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 47.91111111111111,
          "Occur Date": "2020-05-02T00:00:00",
          "Volume": 28
         },
         {
          "90-day": 47.55555555555556,
          "Occur Date": "2020-05-03T00:00:00",
          "Volume": 32
         },
         {
          "90-day": 47.36666666666667,
          "Occur Date": "2020-05-04T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 47.22222222222222,
          "Occur Date": "2020-05-05T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 46.82222222222222,
          "Occur Date": "2020-05-06T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 46.32222222222222,
          "Occur Date": "2020-05-07T00:00:00",
          "Volume": 26
         },
         {
          "90-day": 46.05555555555556,
          "Occur Date": "2020-05-08T00:00:00",
          "Volume": 26
         },
         {
          "90-day": 45.77777777777778,
          "Occur Date": "2020-05-09T00:00:00",
          "Volume": 35
         },
         {
          "90-day": 45.43333333333333,
          "Occur Date": "2020-05-10T00:00:00",
          "Volume": 29
         },
         {
          "90-day": 45.1,
          "Occur Date": "2020-05-11T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 44.82222222222222,
          "Occur Date": "2020-05-12T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 44.48888888888889,
          "Occur Date": "2020-05-13T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 44.166666666666664,
          "Occur Date": "2020-05-14T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 44.17777777777778,
          "Occur Date": "2020-05-15T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 43.977777777777774,
          "Occur Date": "2020-05-16T00:00:00",
          "Volume": 29
         },
         {
          "90-day": 43.63333333333333,
          "Occur Date": "2020-05-17T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 43.46666666666667,
          "Occur Date": "2020-05-18T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 43.2,
          "Occur Date": "2020-05-19T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 43.05555555555556,
          "Occur Date": "2020-05-20T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 42.855555555555554,
          "Occur Date": "2020-05-21T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 42.71111111111111,
          "Occur Date": "2020-05-22T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 42.5,
          "Occur Date": "2020-05-23T00:00:00",
          "Volume": 35
         },
         {
          "90-day": 42.32222222222222,
          "Occur Date": "2020-05-24T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 42.12222222222222,
          "Occur Date": "2020-05-25T00:00:00",
          "Volume": 32
         },
         {
          "90-day": 42.05555555555556,
          "Occur Date": "2020-05-26T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 42.144444444444446,
          "Occur Date": "2020-05-27T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 42.01111111111111,
          "Occur Date": "2020-05-28T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 42.144444444444446,
          "Occur Date": "2020-05-29T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 42.8,
          "Occur Date": "2020-05-30T00:00:00",
          "Volume": 125
         },
         {
          "90-day": 42.62222222222222,
          "Occur Date": "2020-05-31T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 42.6,
          "Occur Date": "2020-06-01T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 42.522222222222226,
          "Occur Date": "2020-06-02T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 42.63333333333333,
          "Occur Date": "2020-06-03T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 42.733333333333334,
          "Occur Date": "2020-06-04T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 42.58888888888889,
          "Occur Date": "2020-06-05T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 42.666666666666664,
          "Occur Date": "2020-06-06T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 42.388888888888886,
          "Occur Date": "2020-06-07T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 42.21111111111111,
          "Occur Date": "2020-06-08T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 42.25555555555555,
          "Occur Date": "2020-06-09T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 42.144444444444446,
          "Occur Date": "2020-06-10T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 42.022222222222226,
          "Occur Date": "2020-06-11T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 41.91111111111111,
          "Occur Date": "2020-06-12T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 41.87777777777778,
          "Occur Date": "2020-06-13T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 41.82222222222222,
          "Occur Date": "2020-06-14T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 42.044444444444444,
          "Occur Date": "2020-06-15T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 42.166666666666664,
          "Occur Date": "2020-06-16T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 42.5,
          "Occur Date": "2020-06-17T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 42.611111111111114,
          "Occur Date": "2020-06-18T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 42.72222222222222,
          "Occur Date": "2020-06-19T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 43.022222222222226,
          "Occur Date": "2020-06-20T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 43.05555555555556,
          "Occur Date": "2020-06-21T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 43.111111111111114,
          "Occur Date": "2020-06-22T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 43.08888888888889,
          "Occur Date": "2020-06-23T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 43.27777777777778,
          "Occur Date": "2020-06-24T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 43.233333333333334,
          "Occur Date": "2020-06-25T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 43.31111111111111,
          "Occur Date": "2020-06-26T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 43.58888888888889,
          "Occur Date": "2020-06-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 43.94444444444444,
          "Occur Date": "2020-06-28T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 44.111111111111114,
          "Occur Date": "2020-06-29T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 44.03333333333333,
          "Occur Date": "2020-06-30T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 44.2,
          "Occur Date": "2020-07-01T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 44.25555555555555,
          "Occur Date": "2020-07-02T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 44.5,
          "Occur Date": "2020-07-03T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 44.888888888888886,
          "Occur Date": "2020-07-04T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 45.25555555555555,
          "Occur Date": "2020-07-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 45.37777777777778,
          "Occur Date": "2020-07-06T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 45.55555555555556,
          "Occur Date": "2020-07-07T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 45.7,
          "Occur Date": "2020-07-08T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 45.93333333333333,
          "Occur Date": "2020-07-09T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 46.03333333333333,
          "Occur Date": "2020-07-10T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 46.28888888888889,
          "Occur Date": "2020-07-11T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 46.34444444444444,
          "Occur Date": "2020-07-12T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 46.477777777777774,
          "Occur Date": "2020-07-13T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 46.32222222222222,
          "Occur Date": "2020-07-14T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 46.5,
          "Occur Date": "2020-07-15T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 46.611111111111114,
          "Occur Date": "2020-07-16T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 46.8,
          "Occur Date": "2020-07-17T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 47.144444444444446,
          "Occur Date": "2020-07-18T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 47.422222222222224,
          "Occur Date": "2020-07-19T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 47.6,
          "Occur Date": "2020-07-20T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 47.67777777777778,
          "Occur Date": "2020-07-21T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 47.8,
          "Occur Date": "2020-07-22T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 47.888888888888886,
          "Occur Date": "2020-07-23T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 48.01111111111111,
          "Occur Date": "2020-07-24T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 48.266666666666666,
          "Occur Date": "2020-07-25T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 48.44444444444444,
          "Occur Date": "2020-07-26T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 48.522222222222226,
          "Occur Date": "2020-07-27T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 48.74444444444445,
          "Occur Date": "2020-07-28T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 48.96666666666667,
          "Occur Date": "2020-07-29T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 49.077777777777776,
          "Occur Date": "2020-07-30T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 49.44444444444444,
          "Occur Date": "2020-07-31T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 49.922222222222224,
          "Occur Date": "2020-08-01T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 49.922222222222224,
          "Occur Date": "2020-08-02T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 50.2,
          "Occur Date": "2020-08-03T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 50.21111111111111,
          "Occur Date": "2020-08-04T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 50.577777777777776,
          "Occur Date": "2020-08-05T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 50.8,
          "Occur Date": "2020-08-06T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 51.12222222222222,
          "Occur Date": "2020-08-07T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 51.36666666666667,
          "Occur Date": "2020-08-08T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 51.7,
          "Occur Date": "2020-08-09T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 51.9,
          "Occur Date": "2020-08-10T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 52.13333333333333,
          "Occur Date": "2020-08-11T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 52.477777777777774,
          "Occur Date": "2020-08-12T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 52.53333333333333,
          "Occur Date": "2020-08-13T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 52.87777777777778,
          "Occur Date": "2020-08-14T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 53.31111111111111,
          "Occur Date": "2020-08-15T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 53.5,
          "Occur Date": "2020-08-16T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 53.544444444444444,
          "Occur Date": "2020-08-17T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 53.62222222222222,
          "Occur Date": "2020-08-18T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 53.81111111111111,
          "Occur Date": "2020-08-19T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 53.766666666666666,
          "Occur Date": "2020-08-20T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 54.06666666666667,
          "Occur Date": "2020-08-21T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 54.24444444444445,
          "Occur Date": "2020-08-22T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 54.48888888888889,
          "Occur Date": "2020-08-23T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.51111111111111,
          "Occur Date": "2020-08-24T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 54.577777777777776,
          "Occur Date": "2020-08-25T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.7,
          "Occur Date": "2020-08-26T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 54.522222222222226,
          "Occur Date": "2020-08-27T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 53.955555555555556,
          "Occur Date": "2020-08-28T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 54.21111111111111,
          "Occur Date": "2020-08-29T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 54.12222222222222,
          "Occur Date": "2020-08-30T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 54.28888888888889,
          "Occur Date": "2020-08-31T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 54.32222222222222,
          "Occur Date": "2020-09-01T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 54.28888888888889,
          "Occur Date": "2020-09-02T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 54.32222222222222,
          "Occur Date": "2020-09-03T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 54.233333333333334,
          "Occur Date": "2020-09-04T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 54.5,
          "Occur Date": "2020-09-05T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 54.644444444444446,
          "Occur Date": "2020-09-06T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 54.766666666666666,
          "Occur Date": "2020-09-07T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 55.12222222222222,
          "Occur Date": "2020-09-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 55.03333333333333,
          "Occur Date": "2020-09-09T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 55.21111111111111,
          "Occur Date": "2020-09-10T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 55.077777777777776,
          "Occur Date": "2020-09-11T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.166666666666664,
          "Occur Date": "2020-09-12T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 55.12222222222222,
          "Occur Date": "2020-09-13T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.111111111111114,
          "Occur Date": "2020-09-14T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 55.2,
          "Occur Date": "2020-09-15T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 55.144444444444446,
          "Occur Date": "2020-09-16T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 55.03333333333333,
          "Occur Date": "2020-09-17T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 55.05555555555556,
          "Occur Date": "2020-09-18T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 55.3,
          "Occur Date": "2020-09-19T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 55.15555555555556,
          "Occur Date": "2020-09-20T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 55.44444444444444,
          "Occur Date": "2020-09-21T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 55.4,
          "Occur Date": "2020-09-22T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 55.544444444444444,
          "Occur Date": "2020-09-23T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 55.53333333333333,
          "Occur Date": "2020-09-24T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.32222222222222,
          "Occur Date": "2020-09-25T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.01111111111111,
          "Occur Date": "2020-09-26T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 55.1,
          "Occur Date": "2020-09-27T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 55.41111111111111,
          "Occur Date": "2020-09-28T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 55.58888888888889,
          "Occur Date": "2020-09-29T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 55.62222222222222,
          "Occur Date": "2020-09-30T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 55.977777777777774,
          "Occur Date": "2020-10-01T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 55.955555555555556,
          "Occur Date": "2020-10-02T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 56.166666666666664,
          "Occur Date": "2020-10-03T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 56.48888888888889,
          "Occur Date": "2020-10-04T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 56.522222222222226,
          "Occur Date": "2020-10-05T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 56.422222222222224,
          "Occur Date": "2020-10-06T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 56.43333333333333,
          "Occur Date": "2020-10-07T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 56.75555555555555,
          "Occur Date": "2020-10-08T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 56.84444444444444,
          "Occur Date": "2020-10-09T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 57.21111111111111,
          "Occur Date": "2020-10-10T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 57.46666666666667,
          "Occur Date": "2020-10-11T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 57.766666666666666,
          "Occur Date": "2020-10-12T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 57.9,
          "Occur Date": "2020-10-13T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 58.2,
          "Occur Date": "2020-10-14T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 58.355555555555554,
          "Occur Date": "2020-10-15T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 58.7,
          "Occur Date": "2020-10-16T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 59.166666666666664,
          "Occur Date": "2020-10-17T00:00:00",
          "Volume": 100
         },
         {
          "90-day": 59.577777777777776,
          "Occur Date": "2020-10-18T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 59.955555555555556,
          "Occur Date": "2020-10-19T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 60.2,
          "Occur Date": "2020-10-20T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 60.51111111111111,
          "Occur Date": "2020-10-21T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 60.46666666666667,
          "Occur Date": "2020-10-22T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 60.6,
          "Occur Date": "2020-10-23T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 61.01111111111111,
          "Occur Date": "2020-10-24T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 61.5,
          "Occur Date": "2020-10-25T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 61.75555555555555,
          "Occur Date": "2020-10-26T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 62.13333333333333,
          "Occur Date": "2020-10-27T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 62.4,
          "Occur Date": "2020-10-28T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 62.72222222222222,
          "Occur Date": "2020-10-29T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 62.62222222222222,
          "Occur Date": "2020-10-30T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.12222222222222,
          "Occur Date": "2020-10-31T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 63.32222222222222,
          "Occur Date": "2020-11-01T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 63.833333333333336,
          "Occur Date": "2020-11-02T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 64.04444444444445,
          "Occur Date": "2020-11-03T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 64.21111111111111,
          "Occur Date": "2020-11-04T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 64.42222222222222,
          "Occur Date": "2020-11-05T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 64.78888888888889,
          "Occur Date": "2020-11-06T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 65.07777777777778,
          "Occur Date": "2020-11-07T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 65.37777777777778,
          "Occur Date": "2020-11-08T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 65.81111111111112,
          "Occur Date": "2020-11-09T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 65.83333333333333,
          "Occur Date": "2020-11-10T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 65.62222222222222,
          "Occur Date": "2020-11-11T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 65.86666666666666,
          "Occur Date": "2020-11-12T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 66.11111111111111,
          "Occur Date": "2020-11-13T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 66.54444444444445,
          "Occur Date": "2020-11-14T00:00:00",
          "Volume": 97
         },
         {
          "90-day": 66.68888888888888,
          "Occur Date": "2020-11-15T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 67.05555555555556,
          "Occur Date": "2020-11-16T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 67.35555555555555,
          "Occur Date": "2020-11-17T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 67.8,
          "Occur Date": "2020-11-18T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 68.1,
          "Occur Date": "2020-11-19T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 68.45555555555555,
          "Occur Date": "2020-11-20T00:00:00",
          "Volume": 91
         },
         {
          "90-day": 68.84444444444445,
          "Occur Date": "2020-11-21T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 69.02222222222223,
          "Occur Date": "2020-11-22T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 69.41111111111111,
          "Occur Date": "2020-11-23T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 69.73333333333333,
          "Occur Date": "2020-11-24T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 70.03333333333333,
          "Occur Date": "2020-11-25T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 69.78888888888889,
          "Occur Date": "2020-11-26T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 69.86666666666666,
          "Occur Date": "2020-11-27T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.24444444444444,
          "Occur Date": "2020-11-28T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 70.4,
          "Occur Date": "2020-11-29T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.58888888888889,
          "Occur Date": "2020-11-30T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 70.84444444444445,
          "Occur Date": "2020-12-01T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.08888888888889,
          "Occur Date": "2020-12-02T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.47777777777777,
          "Occur Date": "2020-12-03T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 71.62222222222222,
          "Occur Date": "2020-12-04T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 71.77777777777777,
          "Occur Date": "2020-12-05T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 71.73333333333333,
          "Occur Date": "2020-12-06T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 71.84444444444445,
          "Occur Date": "2020-12-07T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 72.32222222222222,
          "Occur Date": "2020-12-08T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 72.53333333333333,
          "Occur Date": "2020-12-09T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 72.95555555555555,
          "Occur Date": "2020-12-10T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 73,
          "Occur Date": "2020-12-11T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 73.16666666666667,
          "Occur Date": "2020-12-12T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 73.4,
          "Occur Date": "2020-12-13T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 73.53333333333333,
          "Occur Date": "2020-12-14T00:00:00",
          "Volume": 79
         },
         {
          "90-day": 73.97777777777777,
          "Occur Date": "2020-12-15T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 74.34444444444445,
          "Occur Date": "2020-12-16T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 74.36666666666666,
          "Occur Date": "2020-12-17T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 74.27777777777777,
          "Occur Date": "2020-12-18T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 74.67777777777778,
          "Occur Date": "2020-12-19T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 74.71111111111111,
          "Occur Date": "2020-12-20T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 75.11111111111111,
          "Occur Date": "2020-12-21T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 75.52222222222223,
          "Occur Date": "2020-12-22T00:00:00",
          "Volume": 94
         },
         {
          "90-day": 75.87777777777778,
          "Occur Date": "2020-12-23T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 75.81111111111112,
          "Occur Date": "2020-12-24T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 75.66666666666667,
          "Occur Date": "2020-12-25T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 75.7,
          "Occur Date": "2020-12-26T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 75.55555555555556,
          "Occur Date": "2020-12-27T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 75.84444444444445,
          "Occur Date": "2020-12-28T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 76.06666666666666,
          "Occur Date": "2020-12-29T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 75.67777777777778,
          "Occur Date": "2020-12-30T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 75.28888888888889,
          "Occur Date": "2020-12-31T00:00:00",
          "Volume": 35
         },
         {
          "90-day": 74.95555555555555,
          "Occur Date": "2021-01-01T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 74.72222222222223,
          "Occur Date": "2021-01-02T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 74.62222222222222,
          "Occur Date": "2021-01-03T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 74.67777777777778,
          "Occur Date": "2021-01-04T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 74.62222222222222,
          "Occur Date": "2021-01-05T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 74.42222222222222,
          "Occur Date": "2021-01-06T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 74.22222222222223,
          "Occur Date": "2021-01-07T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 73.9,
          "Occur Date": "2021-01-08T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 73.65555555555555,
          "Occur Date": "2021-01-09T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 73.47777777777777,
          "Occur Date": "2021-01-10T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.5111111111111,
          "Occur Date": "2021-01-11T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 73.26666666666667,
          "Occur Date": "2021-01-12T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 73.26666666666667,
          "Occur Date": "2021-01-13T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 72.84444444444445,
          "Occur Date": "2021-01-14T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 72.36666666666666,
          "Occur Date": "2021-01-15T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 72.08888888888889,
          "Occur Date": "2021-01-16T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.83333333333333,
          "Occur Date": "2021-01-17T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 71.73333333333333,
          "Occur Date": "2021-01-18T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 71.56666666666666,
          "Occur Date": "2021-01-19T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 71.54444444444445,
          "Occur Date": "2021-01-20T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 71.36666666666666,
          "Occur Date": "2021-01-21T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 71.24444444444444,
          "Occur Date": "2021-01-22T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 71.08888888888889,
          "Occur Date": "2021-01-23T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 71.15555555555555,
          "Occur Date": "2021-01-24T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 71.22222222222223,
          "Occur Date": "2021-01-25T00:00:00",
          "Volume": 92
         },
         {
          "90-day": 71.04444444444445,
          "Occur Date": "2021-01-26T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 70.68888888888888,
          "Occur Date": "2021-01-27T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 70.72222222222223,
          "Occur Date": "2021-01-28T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 70.52222222222223,
          "Occur Date": "2021-01-29T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 70.23333333333333,
          "Occur Date": "2021-01-30T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 70.04444444444445,
          "Occur Date": "2021-01-31T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 69.83333333333333,
          "Occur Date": "2021-02-01T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 69.77777777777777,
          "Occur Date": "2021-02-02T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 69.41111111111111,
          "Occur Date": "2021-02-03T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 69.11111111111111,
          "Occur Date": "2021-02-04T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 68.9,
          "Occur Date": "2021-02-05T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 68.5,
          "Occur Date": "2021-02-06T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 67.96666666666667,
          "Occur Date": "2021-02-07T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 67.73333333333333,
          "Occur Date": "2021-02-08T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 67.88888888888889,
          "Occur Date": "2021-02-09T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 67.6,
          "Occur Date": "2021-02-10T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 67.15555555555555,
          "Occur Date": "2021-02-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 66.78888888888889,
          "Occur Date": "2021-02-12T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 66.46666666666667,
          "Occur Date": "2021-02-13T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 66.0111111111111,
          "Occur Date": "2021-02-14T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 65.75555555555556,
          "Occur Date": "2021-02-15T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 65.5111111111111,
          "Occur Date": "2021-02-16T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 65.12222222222222,
          "Occur Date": "2021-02-17T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 64.58888888888889,
          "Occur Date": "2021-02-18T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 64.23333333333333,
          "Occur Date": "2021-02-19T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 64.32222222222222,
          "Occur Date": "2021-02-20T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 64.16666666666667,
          "Occur Date": "2021-02-21T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 63.77777777777778,
          "Occur Date": "2021-02-22T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 63.56666666666667,
          "Occur Date": "2021-02-23T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.65555555555556,
          "Occur Date": "2021-02-24T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.388888888888886,
          "Occur Date": "2021-02-25T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 63.17777777777778,
          "Occur Date": "2021-02-26T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 62.82222222222222,
          "Occur Date": "2021-02-27T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 62.477777777777774,
          "Occur Date": "2021-02-28T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 62.24444444444445,
          "Occur Date": "2021-03-01T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 61.77777777777778,
          "Occur Date": "2021-03-02T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 61.522222222222226,
          "Occur Date": "2021-03-03T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 61.37777777777778,
          "Occur Date": "2021-03-04T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 61.17777777777778,
          "Occur Date": "2021-03-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 61.388888888888886,
          "Occur Date": "2021-03-06T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 61.25555555555555,
          "Occur Date": "2021-03-07T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 60.86666666666667,
          "Occur Date": "2021-03-08T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 60.55555555555556,
          "Occur Date": "2021-03-09T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 60.08888888888889,
          "Occur Date": "2021-03-10T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 59.82222222222222,
          "Occur Date": "2021-03-11T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 59.68888888888889,
          "Occur Date": "2021-03-12T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 59.44444444444444,
          "Occur Date": "2021-03-13T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 59.08888888888889,
          "Occur Date": "2021-03-14T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 58.733333333333334,
          "Occur Date": "2021-03-15T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 58.28888888888889,
          "Occur Date": "2021-03-16T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 57.96666666666667,
          "Occur Date": "2021-03-17T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 57.833333333333336,
          "Occur Date": "2021-03-18T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 57.666666666666664,
          "Occur Date": "2021-03-19T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 57.46666666666667,
          "Occur Date": "2021-03-20T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 57.03333333333333,
          "Occur Date": "2021-03-21T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 56.666666666666664,
          "Occur Date": "2021-03-22T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 56.25555555555555,
          "Occur Date": "2021-03-23T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 56.18888888888889,
          "Occur Date": "2021-03-24T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 56.422222222222224,
          "Occur Date": "2021-03-25T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 56.34444444444444,
          "Occur Date": "2021-03-26T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 56.077777777777776,
          "Occur Date": "2021-03-27T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 55.65555555555556,
          "Occur Date": "2021-03-28T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.477777777777774,
          "Occur Date": "2021-03-29T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 55.544444444444444,
          "Occur Date": "2021-03-30T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 55.766666666666666,
          "Occur Date": "2021-03-31T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 55.666666666666664,
          "Occur Date": "2021-04-01T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 55.666666666666664,
          "Occur Date": "2021-04-02T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.67777777777778,
          "Occur Date": "2021-04-03T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 55.56666666666667,
          "Occur Date": "2021-04-04T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 55.63333333333333,
          "Occur Date": "2021-04-05T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 55.5,
          "Occur Date": "2021-04-06T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.355555555555554,
          "Occur Date": "2021-04-07T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 55.2,
          "Occur Date": "2021-04-08T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 55.17777777777778,
          "Occur Date": "2021-04-09T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 55.2,
          "Occur Date": "2021-04-10T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 55.08888888888889,
          "Occur Date": "2021-04-11T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.077777777777776,
          "Occur Date": "2021-04-12T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 54.78888888888889,
          "Occur Date": "2021-04-13T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 54.77777777777778,
          "Occur Date": "2021-04-14T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 54.62222222222222,
          "Occur Date": "2021-04-15T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 54.522222222222226,
          "Occur Date": "2021-04-16T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 54.5,
          "Occur Date": "2021-04-17T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 54.24444444444445,
          "Occur Date": "2021-04-18T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 54.1,
          "Occur Date": "2021-04-19T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 53.87777777777778,
          "Occur Date": "2021-04-20T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 53.84444444444444,
          "Occur Date": "2021-04-21T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 53.37777777777778,
          "Occur Date": "2021-04-22T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 53.17777777777778,
          "Occur Date": "2021-04-23T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 52.922222222222224,
          "Occur Date": "2021-04-24T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 52.577777777777776,
          "Occur Date": "2021-04-25T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 52.522222222222226,
          "Occur Date": "2021-04-26T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 52.455555555555556,
          "Occur Date": "2021-04-27T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 52.2,
          "Occur Date": "2021-04-28T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 52.022222222222226,
          "Occur Date": "2021-04-29T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 51.96666666666667,
          "Occur Date": "2021-04-30T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 51.84444444444444,
          "Occur Date": "2021-05-01T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 51.82222222222222,
          "Occur Date": "2021-05-02T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 51.78888888888889,
          "Occur Date": "2021-05-03T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 51.733333333333334,
          "Occur Date": "2021-05-04T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 51.77777777777778,
          "Occur Date": "2021-05-05T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 51.65555555555556,
          "Occur Date": "2021-05-06T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 51.65555555555556,
          "Occur Date": "2021-05-07T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 51.81111111111111,
          "Occur Date": "2021-05-08T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 51.733333333333334,
          "Occur Date": "2021-05-09T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 51.63333333333333,
          "Occur Date": "2021-05-10T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 51.388888888888886,
          "Occur Date": "2021-05-11T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 51.15555555555556,
          "Occur Date": "2021-05-12T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 50.86666666666667,
          "Occur Date": "2021-05-13T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 51.044444444444444,
          "Occur Date": "2021-05-14T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 51.43333333333333,
          "Occur Date": "2021-05-15T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 51.34444444444444,
          "Occur Date": "2021-05-16T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 51.56666666666667,
          "Occur Date": "2021-05-17T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 51.577777777777776,
          "Occur Date": "2021-05-18T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 51.522222222222226,
          "Occur Date": "2021-05-19T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 51.34444444444444,
          "Occur Date": "2021-05-20T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 51.12222222222222,
          "Occur Date": "2021-05-21T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 50.98888888888889,
          "Occur Date": "2021-05-22T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 50.91111111111111,
          "Occur Date": "2021-05-23T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 50.74444444444445,
          "Occur Date": "2021-05-24T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 50.55555555555556,
          "Occur Date": "2021-05-25T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 50.5,
          "Occur Date": "2021-05-26T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 50.266666666666666,
          "Occur Date": "2021-05-27T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 50.25555555555555,
          "Occur Date": "2021-05-28T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 50.455555555555556,
          "Occur Date": "2021-05-29T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 50.477777777777774,
          "Occur Date": "2021-05-30T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 50.53333333333333,
          "Occur Date": "2021-05-31T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 50.266666666666666,
          "Occur Date": "2021-06-01T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 50.022222222222226,
          "Occur Date": "2021-06-02T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 49.94444444444444,
          "Occur Date": "2021-06-03T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 49.93333333333333,
          "Occur Date": "2021-06-04T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 49.833333333333336,
          "Occur Date": "2021-06-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 49.65555555555556,
          "Occur Date": "2021-06-06T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 49.577777777777776,
          "Occur Date": "2021-06-07T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 49.577777777777776,
          "Occur Date": "2021-06-08T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 49.51111111111111,
          "Occur Date": "2021-06-09T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 49.44444444444444,
          "Occur Date": "2021-06-10T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 49.522222222222226,
          "Occur Date": "2021-06-11T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 49.62222222222222,
          "Occur Date": "2021-06-12T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 49.58888888888889,
          "Occur Date": "2021-06-13T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 49.766666666666666,
          "Occur Date": "2021-06-14T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 49.9,
          "Occur Date": "2021-06-15T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 49.86666666666667,
          "Occur Date": "2021-06-16T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 49.75555555555555,
          "Occur Date": "2021-06-17T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 49.75555555555555,
          "Occur Date": "2021-06-18T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 49.74444444444445,
          "Occur Date": "2021-06-19T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 49.455555555555556,
          "Occur Date": "2021-06-20T00:00:00",
          "Volume": 35
         },
         {
          "90-day": 49.48888888888889,
          "Occur Date": "2021-06-21T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 49.455555555555556,
          "Occur Date": "2021-06-22T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 49.422222222222224,
          "Occur Date": "2021-06-23T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 49.28888888888889,
          "Occur Date": "2021-06-24T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 49.522222222222226,
          "Occur Date": "2021-06-25T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 49.544444444444444,
          "Occur Date": "2021-06-26T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 49.6,
          "Occur Date": "2021-06-27T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 49.65555555555556,
          "Occur Date": "2021-06-28T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 49.82222222222222,
          "Occur Date": "2021-06-29T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 50.05555555555556,
          "Occur Date": "2021-06-30T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 50.17777777777778,
          "Occur Date": "2021-07-01T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 50.3,
          "Occur Date": "2021-07-02T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 50.55555555555556,
          "Occur Date": "2021-07-03T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 50.65555555555556,
          "Occur Date": "2021-07-04T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 50.766666666666666,
          "Occur Date": "2021-07-05T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 50.94444444444444,
          "Occur Date": "2021-07-06T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 51.31111111111111,
          "Occur Date": "2021-07-07T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 51.144444444444446,
          "Occur Date": "2021-07-08T00:00:00",
          "Volume": 38
         },
         {
          "90-day": 51.166666666666664,
          "Occur Date": "2021-07-09T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 51.27777777777778,
          "Occur Date": "2021-07-10T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 51.32222222222222,
          "Occur Date": "2021-07-11T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 51.51111111111111,
          "Occur Date": "2021-07-12T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 51.75555555555555,
          "Occur Date": "2021-07-13T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 51.8,
          "Occur Date": "2021-07-14T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 52.05555555555556,
          "Occur Date": "2021-07-15T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 52.12222222222222,
          "Occur Date": "2021-07-16T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 52.388888888888886,
          "Occur Date": "2021-07-17T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 52.56666666666667,
          "Occur Date": "2021-07-18T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 52.91111111111111,
          "Occur Date": "2021-07-19T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 53.044444444444444,
          "Occur Date": "2021-07-20T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 53.28888888888889,
          "Occur Date": "2021-07-21T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 53.46666666666667,
          "Occur Date": "2021-07-22T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 53.55555555555556,
          "Occur Date": "2021-07-23T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 53.6,
          "Occur Date": "2021-07-24T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 53.75555555555555,
          "Occur Date": "2021-07-25T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 53.87777777777778,
          "Occur Date": "2021-07-26T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 53.96666666666667,
          "Occur Date": "2021-07-27T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54,
          "Occur Date": "2021-07-28T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 54.077777777777776,
          "Occur Date": "2021-07-29T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 54.28888888888889,
          "Occur Date": "2021-07-30T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 54.44444444444444,
          "Occur Date": "2021-07-31T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 54.77777777777778,
          "Occur Date": "2021-08-01T00:00:00",
          "Volume": 83
         },
         {
          "90-day": 55.01111111111111,
          "Occur Date": "2021-08-02T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 54.955555555555556,
          "Occur Date": "2021-08-03T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 54.93333333333333,
          "Occur Date": "2021-08-04T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 55.05555555555556,
          "Occur Date": "2021-08-05T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 55.08888888888889,
          "Occur Date": "2021-08-06T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 55.577777777777776,
          "Occur Date": "2021-08-07T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 55.74444444444445,
          "Occur Date": "2021-08-08T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 56.1,
          "Occur Date": "2021-08-09T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 56.22222222222222,
          "Occur Date": "2021-08-10T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 56.41111111111111,
          "Occur Date": "2021-08-11T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 56.3,
          "Occur Date": "2021-08-12T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 56.18888888888889,
          "Occur Date": "2021-08-13T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 56.455555555555556,
          "Occur Date": "2021-08-14T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 56.455555555555556,
          "Occur Date": "2021-08-15T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 56.522222222222226,
          "Occur Date": "2021-08-16T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 56.72222222222222,
          "Occur Date": "2021-08-17T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 56.93333333333333,
          "Occur Date": "2021-08-18T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 57.06666666666667,
          "Occur Date": "2021-08-19T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 56.888888888888886,
          "Occur Date": "2021-08-20T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 57.18888888888889,
          "Occur Date": "2021-08-21T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 57.31111111111111,
          "Occur Date": "2021-08-22T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 57.55555555555556,
          "Occur Date": "2021-08-23T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 57.72222222222222,
          "Occur Date": "2021-08-24T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 58,
          "Occur Date": "2021-08-25T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 58,
          "Occur Date": "2021-08-26T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 57.86666666666667,
          "Occur Date": "2021-08-27T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 57.93333333333333,
          "Occur Date": "2021-08-28T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 58.28888888888889,
          "Occur Date": "2021-08-29T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 58.68888888888889,
          "Occur Date": "2021-08-30T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 59.022222222222226,
          "Occur Date": "2021-08-31T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 59.15555555555556,
          "Occur Date": "2021-09-01T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 59.03333333333333,
          "Occur Date": "2021-09-02T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 59.12222222222222,
          "Occur Date": "2021-09-03T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 59.55555555555556,
          "Occur Date": "2021-09-04T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 59.82222222222222,
          "Occur Date": "2021-09-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 60.17777777777778,
          "Occur Date": "2021-09-06T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 60.5,
          "Occur Date": "2021-09-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 60.58888888888889,
          "Occur Date": "2021-09-08T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 60.71111111111111,
          "Occur Date": "2021-09-09T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 60.67777777777778,
          "Occur Date": "2021-09-10T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 60.922222222222224,
          "Occur Date": "2021-09-11T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 61.21111111111111,
          "Occur Date": "2021-09-12T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 61.266666666666666,
          "Occur Date": "2021-09-13T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 61.37777777777778,
          "Occur Date": "2021-09-14T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 61.455555555555556,
          "Occur Date": "2021-09-15T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 61.36666666666667,
          "Occur Date": "2021-09-16T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 61.71111111111111,
          "Occur Date": "2021-09-17T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 61.96666666666667,
          "Occur Date": "2021-09-18T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 62.111111111111114,
          "Occur Date": "2021-09-19T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 62.37777777777778,
          "Occur Date": "2021-09-20T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 62.455555555555556,
          "Occur Date": "2021-09-21T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 62.71111111111111,
          "Occur Date": "2021-09-22T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 62.53333333333333,
          "Occur Date": "2021-09-23T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 62.91111111111111,
          "Occur Date": "2021-09-24T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 62.733333333333334,
          "Occur Date": "2021-09-25T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 62.81111111111111,
          "Occur Date": "2021-09-26T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 62.733333333333334,
          "Occur Date": "2021-09-27T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 62.644444444444446,
          "Occur Date": "2021-09-28T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 62.7,
          "Occur Date": "2021-09-29T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 62.55555555555556,
          "Occur Date": "2021-09-30T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 62.34444444444444,
          "Occur Date": "2021-10-01T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 62.27777777777778,
          "Occur Date": "2021-10-02T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 62.32222222222222,
          "Occur Date": "2021-10-03T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 62.266666666666666,
          "Occur Date": "2021-10-04T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 62,
          "Occur Date": "2021-10-05T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 62.17777777777778,
          "Occur Date": "2021-10-06T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 62.17777777777778,
          "Occur Date": "2021-10-07T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 62.1,
          "Occur Date": "2021-10-08T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 62.25555555555555,
          "Occur Date": "2021-10-09T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 62.43333333333333,
          "Occur Date": "2021-10-10T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 62.46666666666667,
          "Occur Date": "2021-10-11T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 62.87777777777778,
          "Occur Date": "2021-10-12T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 62.84444444444444,
          "Occur Date": "2021-10-13T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 62.922222222222224,
          "Occur Date": "2021-10-14T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.022222222222226,
          "Occur Date": "2021-10-15T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 63.06666666666667,
          "Occur Date": "2021-10-16T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 62.91111111111111,
          "Occur Date": "2021-10-17T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 63.12222222222222,
          "Occur Date": "2021-10-18T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 63.22222222222222,
          "Occur Date": "2021-10-19T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 63.03333333333333,
          "Occur Date": "2021-10-20T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 62.91111111111111,
          "Occur Date": "2021-10-21T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 62.955555555555556,
          "Occur Date": "2021-10-22T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 62.96666666666667,
          "Occur Date": "2021-10-23T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 62.93333333333333,
          "Occur Date": "2021-10-24T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.044444444444444,
          "Occur Date": "2021-10-25T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 62.977777777777774,
          "Occur Date": "2021-10-26T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 62.96666666666667,
          "Occur Date": "2021-10-27T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 62.68888888888889,
          "Occur Date": "2021-10-28T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 62.74444444444445,
          "Occur Date": "2021-10-29T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 62.733333333333334,
          "Occur Date": "2021-10-30T00:00:00",
          "Volume": 82
         },
         {
          "90-day": 62.955555555555556,
          "Occur Date": "2021-10-31T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 63.166666666666664,
          "Occur Date": "2021-11-01T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 63.166666666666664,
          "Occur Date": "2021-11-02T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 63.25555555555555,
          "Occur Date": "2021-11-03T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 63.266666666666666,
          "Occur Date": "2021-11-04T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 63,
          "Occur Date": "2021-11-05T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 63.05555555555556,
          "Occur Date": "2021-11-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 63.05555555555556,
          "Occur Date": "2021-11-07T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.32222222222222,
          "Occur Date": "2021-11-08T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 63.455555555555556,
          "Occur Date": "2021-11-09T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 63.46666666666667,
          "Occur Date": "2021-11-10T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 63.4,
          "Occur Date": "2021-11-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 63.2,
          "Occur Date": "2021-11-12T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 63.25555555555555,
          "Occur Date": "2021-11-13T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 63.233333333333334,
          "Occur Date": "2021-11-14T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 63.34444444444444,
          "Occur Date": "2021-11-15T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.25555555555555,
          "Occur Date": "2021-11-16T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 63.18888888888889,
          "Occur Date": "2021-11-17T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 63.355555555555554,
          "Occur Date": "2021-11-18T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 63.48888888888889,
          "Occur Date": "2021-11-19T00:00:00",
          "Volume": 76
         },
         {
          "90-day": 63.62222222222222,
          "Occur Date": "2021-11-20T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 63.6,
          "Occur Date": "2021-11-21T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 63.55555555555556,
          "Occur Date": "2021-11-22T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 63.41111111111111,
          "Occur Date": "2021-11-23T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 63.51111111111111,
          "Occur Date": "2021-11-24T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 63.31111111111111,
          "Occur Date": "2021-11-25T00:00:00",
          "Volume": 33
         },
         {
          "90-day": 63.455555555555556,
          "Occur Date": "2021-11-26T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 63.53333333333333,
          "Occur Date": "2021-11-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 63.455555555555556,
          "Occur Date": "2021-11-28T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 63.55555555555556,
          "Occur Date": "2021-11-29T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 63.577777777777776,
          "Occur Date": "2021-11-30T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 63.63333333333333,
          "Occur Date": "2021-12-01T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 63.522222222222226,
          "Occur Date": "2021-12-02T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.44444444444444,
          "Occur Date": "2021-12-03T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 63.6,
          "Occur Date": "2021-12-04T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 63.7,
          "Occur Date": "2021-12-05T00:00:00",
          "Volume": 86
         },
         {
          "90-day": 63.733333333333334,
          "Occur Date": "2021-12-06T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 63.7,
          "Occur Date": "2021-12-07T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.5,
          "Occur Date": "2021-12-08T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 63.48888888888889,
          "Occur Date": "2021-12-09T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 63.48888888888889,
          "Occur Date": "2021-12-10T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 63.4,
          "Occur Date": "2021-12-11T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 63.58888888888889,
          "Occur Date": "2021-12-12T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 63.62222222222222,
          "Occur Date": "2021-12-13T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 63.58888888888889,
          "Occur Date": "2021-12-14T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 63.888888888888886,
          "Occur Date": "2021-12-15T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 63.666666666666664,
          "Occur Date": "2021-12-16T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 64.02222222222223,
          "Occur Date": "2021-12-17T00:00:00",
          "Volume": 90
         },
         {
          "90-day": 64.15555555555555,
          "Occur Date": "2021-12-18T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 64.08888888888889,
          "Occur Date": "2021-12-19T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 64.16666666666667,
          "Occur Date": "2021-12-20T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.87777777777778,
          "Occur Date": "2021-12-21T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 64.25555555555556,
          "Occur Date": "2021-12-22T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 63.833333333333336,
          "Occur Date": "2021-12-23T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 64,
          "Occur Date": "2021-12-24T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 63.644444444444446,
          "Occur Date": "2021-12-25T00:00:00",
          "Volume": 27
         },
         {
          "90-day": 63.4,
          "Occur Date": "2021-12-26T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 63.34444444444444,
          "Occur Date": "2021-12-27T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 63.022222222222226,
          "Occur Date": "2021-12-28T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 63.022222222222226,
          "Occur Date": "2021-12-29T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 62.7,
          "Occur Date": "2021-12-30T00:00:00",
          "Volume": 14
         },
         {
          "90-day": 62.233333333333334,
          "Occur Date": "2021-12-31T00:00:00",
          "Volume": 22
         },
         {
          "90-day": 61.977777777777774,
          "Occur Date": "2022-01-01T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 61.855555555555554,
          "Occur Date": "2022-01-02T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 61.87777777777778,
          "Occur Date": "2022-01-03T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 61.81111111111111,
          "Occur Date": "2022-01-04T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 61.78888888888889,
          "Occur Date": "2022-01-05T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 61.7,
          "Occur Date": "2022-01-06T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 61.522222222222226,
          "Occur Date": "2022-01-07T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 61.4,
          "Occur Date": "2022-01-08T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 61.233333333333334,
          "Occur Date": "2022-01-09T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 60.888888888888886,
          "Occur Date": "2022-01-10T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 60.477777777777774,
          "Occur Date": "2022-01-11T00:00:00",
          "Volume": 29
         },
         {
          "90-day": 60.4,
          "Occur Date": "2022-01-12T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 60.044444444444444,
          "Occur Date": "2022-01-13T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 59.8,
          "Occur Date": "2022-01-14T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 59.87777777777778,
          "Occur Date": "2022-01-15T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 59.62222222222222,
          "Occur Date": "2022-01-16T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 59.522222222222226,
          "Occur Date": "2022-01-17T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 59.58888888888889,
          "Occur Date": "2022-01-18T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 59.51111111111111,
          "Occur Date": "2022-01-19T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 59.28888888888889,
          "Occur Date": "2022-01-20T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 59.21111111111111,
          "Occur Date": "2022-01-21T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 59.3,
          "Occur Date": "2022-01-22T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 59.03333333333333,
          "Occur Date": "2022-01-23T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 59.03333333333333,
          "Occur Date": "2022-01-24T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 58.82222222222222,
          "Occur Date": "2022-01-25T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 58.74444444444445,
          "Occur Date": "2022-01-26T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 58.355555555555554,
          "Occur Date": "2022-01-27T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 57.96666666666667,
          "Occur Date": "2022-01-28T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 57.68888888888889,
          "Occur Date": "2022-01-29T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 57.48888888888889,
          "Occur Date": "2022-01-30T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 57.36666666666667,
          "Occur Date": "2022-01-31T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 57.111111111111114,
          "Occur Date": "2022-02-01T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 56.9,
          "Occur Date": "2022-02-02T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 56.63333333333333,
          "Occur Date": "2022-02-03T00:00:00",
          "Volume": 34
         },
         {
          "90-day": 56.62222222222222,
          "Occur Date": "2022-02-04T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 56.455555555555556,
          "Occur Date": "2022-02-05T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 56.36666666666667,
          "Occur Date": "2022-02-06T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 56.18888888888889,
          "Occur Date": "2022-02-07T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 56.08888888888889,
          "Occur Date": "2022-02-08T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 56.03333333333333,
          "Occur Date": "2022-02-09T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 56.044444444444444,
          "Occur Date": "2022-02-10T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 55.977777777777774,
          "Occur Date": "2022-02-11T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 56.1,
          "Occur Date": "2022-02-12T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 56.13333333333333,
          "Occur Date": "2022-02-13T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 56.166666666666664,
          "Occur Date": "2022-02-14T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 55.98888888888889,
          "Occur Date": "2022-02-15T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 56.03333333333333,
          "Occur Date": "2022-02-16T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 55.733333333333334,
          "Occur Date": "2022-02-17T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 55.58888888888889,
          "Occur Date": "2022-02-18T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 55.522222222222226,
          "Occur Date": "2022-02-19T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 55.77777777777778,
          "Occur Date": "2022-02-20T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 55.733333333333334,
          "Occur Date": "2022-02-21T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 55.666666666666664,
          "Occur Date": "2022-02-22T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 55.94444444444444,
          "Occur Date": "2022-02-23T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.75555555555555,
          "Occur Date": "2022-02-24T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.388888888888886,
          "Occur Date": "2022-02-25T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 55.27777777777778,
          "Occur Date": "2022-02-26T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.17777777777778,
          "Occur Date": "2022-02-27T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 54.98888888888889,
          "Occur Date": "2022-02-28T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 54.84444444444444,
          "Occur Date": "2022-03-01T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 54.78888888888889,
          "Occur Date": "2022-03-02T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 54.44444444444444,
          "Occur Date": "2022-03-03T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 54.111111111111114,
          "Occur Date": "2022-03-04T00:00:00",
          "Volume": 55
         },
         {
          "90-day": 53.94444444444444,
          "Occur Date": "2022-03-05T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 53.78888888888889,
          "Occur Date": "2022-03-06T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 53.577777777777776,
          "Occur Date": "2022-03-07T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 53.44444444444444,
          "Occur Date": "2022-03-08T00:00:00",
          "Volume": 37
         },
         {
          "90-day": 53.51111111111111,
          "Occur Date": "2022-03-09T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 53.28888888888889,
          "Occur Date": "2022-03-10T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 53.06666666666667,
          "Occur Date": "2022-03-11T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 52.82222222222222,
          "Occur Date": "2022-03-12T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 52.611111111111114,
          "Occur Date": "2022-03-13T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 52.68888888888889,
          "Occur Date": "2022-03-14T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 52.522222222222226,
          "Occur Date": "2022-03-15T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 52.51111111111111,
          "Occur Date": "2022-03-16T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 51.91111111111111,
          "Occur Date": "2022-03-17T00:00:00",
          "Volume": 36
         },
         {
          "90-day": 51.7,
          "Occur Date": "2022-03-18T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 51.58888888888889,
          "Occur Date": "2022-03-19T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 51.544444444444444,
          "Occur Date": "2022-03-20T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 51.74444444444445,
          "Occur Date": "2022-03-21T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 51.388888888888886,
          "Occur Date": "2022-03-22T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 51.44444444444444,
          "Occur Date": "2022-03-23T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 51.333333333333336,
          "Occur Date": "2022-03-24T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 51.733333333333334,
          "Occur Date": "2022-03-25T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 51.84444444444444,
          "Occur Date": "2022-03-26T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 51.87777777777778,
          "Occur Date": "2022-03-27T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 51.833333333333336,
          "Occur Date": "2022-03-28T00:00:00",
          "Volume": 41
         },
         {
          "90-day": 51.87777777777778,
          "Occur Date": "2022-03-29T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 52.31111111111111,
          "Occur Date": "2022-03-30T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 52.666666666666664,
          "Occur Date": "2022-03-31T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 52.75555555555555,
          "Occur Date": "2022-04-01T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 52.922222222222224,
          "Occur Date": "2022-04-02T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 52.9,
          "Occur Date": "2022-04-03T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 53.06666666666667,
          "Occur Date": "2022-04-04T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 52.91111111111111,
          "Occur Date": "2022-04-05T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 53.044444444444444,
          "Occur Date": "2022-04-06T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 53.022222222222226,
          "Occur Date": "2022-04-07T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 53.022222222222226,
          "Occur Date": "2022-04-08T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 52.96666666666667,
          "Occur Date": "2022-04-09T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 53.1,
          "Occur Date": "2022-04-10T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 53.4,
          "Occur Date": "2022-04-11T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 53.266666666666666,
          "Occur Date": "2022-04-12T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 53.477777777777774,
          "Occur Date": "2022-04-13T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 53.44444444444444,
          "Occur Date": "2022-04-14T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 53.5,
          "Occur Date": "2022-04-15T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 53.666666666666664,
          "Occur Date": "2022-04-16T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 53.56666666666667,
          "Occur Date": "2022-04-17T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 53.41111111111111,
          "Occur Date": "2022-04-18T00:00:00",
          "Volume": 45
         },
         {
          "90-day": 53.53333333333333,
          "Occur Date": "2022-04-19T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 53.522222222222226,
          "Occur Date": "2022-04-20T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 53.455555555555556,
          "Occur Date": "2022-04-21T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 53.333333333333336,
          "Occur Date": "2022-04-22T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 53.62222222222222,
          "Occur Date": "2022-04-23T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 53.56666666666667,
          "Occur Date": "2022-04-24T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 53.68888888888889,
          "Occur Date": "2022-04-25T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 53.766666666666666,
          "Occur Date": "2022-04-26T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 53.8,
          "Occur Date": "2022-04-27T00:00:00",
          "Volume": 44
         },
         {
          "90-day": 53.955555555555556,
          "Occur Date": "2022-04-28T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 54,
          "Occur Date": "2022-04-29T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 54.18888888888889,
          "Occur Date": "2022-04-30T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 54.27777777777778,
          "Occur Date": "2022-05-01T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.46666666666667,
          "Occur Date": "2022-05-02T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 54.6,
          "Occur Date": "2022-05-03T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 54.81111111111111,
          "Occur Date": "2022-05-04T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 54.7,
          "Occur Date": "2022-05-05T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 54.766666666666666,
          "Occur Date": "2022-05-06T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 54.666666666666664,
          "Occur Date": "2022-05-07T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.766666666666666,
          "Occur Date": "2022-05-08T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 54.84444444444444,
          "Occur Date": "2022-05-09T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 54.922222222222224,
          "Occur Date": "2022-05-10T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 54.8,
          "Occur Date": "2022-05-11T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 54.544444444444444,
          "Occur Date": "2022-05-12T00:00:00",
          "Volume": 48
         },
         {
          "90-day": 54.4,
          "Occur Date": "2022-05-13T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 54.233333333333334,
          "Occur Date": "2022-05-14T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.355555555555554,
          "Occur Date": "2022-05-15T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 54.522222222222226,
          "Occur Date": "2022-05-16T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 54.25555555555555,
          "Occur Date": "2022-05-17T00:00:00",
          "Volume": 42
         },
         {
          "90-day": 54.233333333333334,
          "Occur Date": "2022-05-18T00:00:00",
          "Volume": 47
         },
         {
          "90-day": 54.15555555555556,
          "Occur Date": "2022-05-19T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 54.2,
          "Occur Date": "2022-05-20T00:00:00",
          "Volume": 61
         },
         {
          "90-day": 54.233333333333334,
          "Occur Date": "2022-05-21T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 54.43333333333333,
          "Occur Date": "2022-05-22T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 54.67777777777778,
          "Occur Date": "2022-05-23T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 54.77777777777778,
          "Occur Date": "2022-05-24T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 54.833333333333336,
          "Occur Date": "2022-05-25T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 54.98888888888889,
          "Occur Date": "2022-05-26T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 55.233333333333334,
          "Occur Date": "2022-05-27T00:00:00",
          "Volume": 80
         },
         {
          "90-day": 55.166666666666664,
          "Occur Date": "2022-05-28T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 55.44444444444444,
          "Occur Date": "2022-05-29T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 55.46666666666667,
          "Occur Date": "2022-05-30T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 55.56666666666667,
          "Occur Date": "2022-05-31T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 55.855555555555554,
          "Occur Date": "2022-06-01T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 56.06666666666667,
          "Occur Date": "2022-06-02T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 56.05555555555556,
          "Occur Date": "2022-06-03T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 55.855555555555554,
          "Occur Date": "2022-06-04T00:00:00",
          "Volume": 39
         },
         {
          "90-day": 56.144444444444446,
          "Occur Date": "2022-06-05T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 56.53333333333333,
          "Occur Date": "2022-06-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 56.477777777777774,
          "Occur Date": "2022-06-07T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 56.5,
          "Occur Date": "2022-06-08T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 56.522222222222226,
          "Occur Date": "2022-06-09T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 56.733333333333334,
          "Occur Date": "2022-06-10T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 56.9,
          "Occur Date": "2022-06-11T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 56.96666666666667,
          "Occur Date": "2022-06-12T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 56.9,
          "Occur Date": "2022-06-13T00:00:00",
          "Volume": 53
         },
         {
          "90-day": 56.977777777777774,
          "Occur Date": "2022-06-14T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 57.15555555555556,
          "Occur Date": "2022-06-15T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 57.166666666666664,
          "Occur Date": "2022-06-16T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 57.25555555555555,
          "Occur Date": "2022-06-17T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 57.46666666666667,
          "Occur Date": "2022-06-18T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 57.58888888888889,
          "Occur Date": "2022-06-19T00:00:00",
          "Volume": 71
         },
         {
          "90-day": 57.77777777777778,
          "Occur Date": "2022-06-20T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 57.94444444444444,
          "Occur Date": "2022-06-21T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 58.12222222222222,
          "Occur Date": "2022-06-22T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 58.12222222222222,
          "Occur Date": "2022-06-23T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 58.522222222222226,
          "Occur Date": "2022-06-24T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 58.87777777777778,
          "Occur Date": "2022-06-25T00:00:00",
          "Volume": 85
         },
         {
          "90-day": 59.144444444444446,
          "Occur Date": "2022-06-26T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 59.4,
          "Occur Date": "2022-06-27T00:00:00",
          "Volume": 77
         },
         {
          "90-day": 59.477777777777774,
          "Occur Date": "2022-06-28T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 59.644444444444446,
          "Occur Date": "2022-06-29T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 59.977777777777774,
          "Occur Date": "2022-06-30T00:00:00",
          "Volume": 78
         },
         {
          "90-day": 60.166666666666664,
          "Occur Date": "2022-07-01T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 60.34444444444444,
          "Occur Date": "2022-07-02T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 60.34444444444444,
          "Occur Date": "2022-07-03T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 60.46666666666667,
          "Occur Date": "2022-07-04T00:00:00",
          "Volume": 51
         },
         {
          "90-day": 60.577777777777776,
          "Occur Date": "2022-07-05T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 60.56666666666667,
          "Occur Date": "2022-07-06T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 60.6,
          "Occur Date": "2022-07-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 60.855555555555554,
          "Occur Date": "2022-07-08T00:00:00",
          "Volume": 73
         },
         {
          "90-day": 60.87777777777778,
          "Occur Date": "2022-07-09T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.01111111111111,
          "Occur Date": "2022-07-10T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 61.455555555555556,
          "Occur Date": "2022-07-11T00:00:00",
          "Volume": 87
         },
         {
          "90-day": 61.55555555555556,
          "Occur Date": "2022-07-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 61.666666666666664,
          "Occur Date": "2022-07-13T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 61.522222222222226,
          "Occur Date": "2022-07-14T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 61.55555555555556,
          "Occur Date": "2022-07-15T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 62.03333333333333,
          "Occur Date": "2022-07-16T00:00:00",
          "Volume": 89
         },
         {
          "90-day": 62.333333333333336,
          "Occur Date": "2022-07-17T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 62.44444444444444,
          "Occur Date": "2022-07-18T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 62.455555555555556,
          "Occur Date": "2022-07-19T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 62.611111111111114,
          "Occur Date": "2022-07-20T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 62.87777777777778,
          "Occur Date": "2022-07-21T00:00:00",
          "Volume": 81
         },
         {
          "90-day": 62.922222222222224,
          "Occur Date": "2022-07-22T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 63.044444444444444,
          "Occur Date": "2022-07-23T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 63.111111111111114,
          "Occur Date": "2022-07-24T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 63.455555555555556,
          "Occur Date": "2022-07-25T00:00:00",
          "Volume": 84
         },
         {
          "90-day": 63.63333333333333,
          "Occur Date": "2022-07-26T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.68888888888889,
          "Occur Date": "2022-07-27T00:00:00",
          "Volume": 66
         },
         {
          "90-day": 63.51111111111111,
          "Occur Date": "2022-07-28T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 63.355555555555554,
          "Occur Date": "2022-07-29T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 63.55555555555556,
          "Occur Date": "2022-07-30T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 63.477777777777774,
          "Occur Date": "2022-07-31T00:00:00",
          "Volume": 57
         },
         {
          "90-day": 63.611111111111114,
          "Occur Date": "2022-08-01T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 63.67777777777778,
          "Occur Date": "2022-08-02T00:00:00",
          "Volume": 59
         },
         {
          "90-day": 63.55555555555556,
          "Occur Date": "2022-08-03T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 63.644444444444446,
          "Occur Date": "2022-08-04T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 63.644444444444446,
          "Occur Date": "2022-08-05T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 63.77777777777778,
          "Occur Date": "2022-08-06T00:00:00",
          "Volume": 72
         },
         {
          "90-day": 63.977777777777774,
          "Occur Date": "2022-08-07T00:00:00",
          "Volume": 68
         },
         {
          "90-day": 64.02222222222223,
          "Occur Date": "2022-08-08T00:00:00",
          "Volume": 63
         },
         {
          "90-day": 64.28888888888889,
          "Occur Date": "2022-08-09T00:00:00",
          "Volume": 70
         },
         {
          "90-day": 64.46666666666667,
          "Occur Date": "2022-08-10T00:00:00",
          "Volume": 64
         },
         {
          "90-day": 64.38888888888889,
          "Occur Date": "2022-08-11T00:00:00",
          "Volume": 50
         },
         {
          "90-day": 64.53333333333333,
          "Occur Date": "2022-08-12T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 64.62222222222222,
          "Occur Date": "2022-08-13T00:00:00",
          "Volume": 74
         },
         {
          "90-day": 64.5111111111111,
          "Occur Date": "2022-08-14T00:00:00",
          "Volume": 46
         },
         {
          "90-day": 64.64444444444445,
          "Occur Date": "2022-08-15T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 64.86666666666666,
          "Occur Date": "2022-08-16T00:00:00",
          "Volume": 67
         },
         {
          "90-day": 64.88888888888889,
          "Occur Date": "2022-08-17T00:00:00",
          "Volume": 56
         },
         {
          "90-day": 64.97777777777777,
          "Occur Date": "2022-08-18T00:00:00",
          "Volume": 69
         },
         {
          "90-day": 64.73333333333333,
          "Occur Date": "2022-08-19T00:00:00",
          "Volume": 58
         },
         {
          "90-day": 65,
          "Occur Date": "2022-08-20T00:00:00",
          "Volume": 93
         },
         {
          "90-day": 64.72222222222223,
          "Occur Date": "2022-08-21T00:00:00",
          "Volume": 43
         },
         {
          "90-day": 64.7,
          "Occur Date": "2022-08-22T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 64.68888888888888,
          "Occur Date": "2022-08-23T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 64.73333333333333,
          "Occur Date": "2022-08-24T00:00:00",
          "Volume": 62
         },
         {
          "90-day": 64.5111111111111,
          "Occur Date": "2022-08-25T00:00:00",
          "Volume": 60
         },
         {
          "90-day": 64.36666666666666,
          "Occur Date": "2022-08-26T00:00:00",
          "Volume": 52
         },
         {
          "90-day": 64.34444444444445,
          "Occur Date": "2022-08-27T00:00:00",
          "Volume": 75
         },
         {
          "90-day": 64.31111111111112,
          "Occur Date": "2022-08-28T00:00:00",
          "Volume": 54
         },
         {
          "90-day": 64.32222222222222,
          "Occur Date": "2022-08-29T00:00:00",
          "Volume": 65
         },
         {
          "90-day": 64,
          "Occur Date": "2022-08-30T00:00:00",
          "Volume": 40
         },
         {
          "90-day": 63.72222222222222,
          "Occur Date": "2022-08-31T00:00:00",
          "Volume": 49
         },
         {
          "90-day": 63.01111111111111,
          "Occur Date": "2022-09-01T00:00:00",
          "Volume": 6
         }
        ]
       },
       "vconcat": [
        {
         "data": {
          "name": "data-f11de618d66d4277a24c982189e85d20"
         },
         "layer": [
          {
           "encoding": {
            "color": {
             "value": "#7CB9E8"
            },
            "x": {
             "field": "Occur Date",
             "type": "temporal"
            },
            "y": {
             "field": "Volume",
             "type": "quantitative"
            }
           },
           "height": 300,
           "mark": "area",
           "selection": {
            "selector001": {
             "bind": "scales",
             "encodings": [
              "x",
              "y"
             ],
             "type": "interval"
            }
           },
           "title": "Crime Volume Over Time",
           "width": 800
          },
          {
           "encoding": {
            "color": {
             "value": "#FF0808"
            },
            "tooltip": {
             "field": "Volume",
             "type": "quantitative"
            },
            "x": {
             "field": "Occur Date",
             "type": "temporal"
            },
            "y": {
             "field": "90-day",
             "type": "quantitative"
            }
           },
           "mark": "line"
          }
         ]
        },
        {
         "data": {
          "name": "data-1c7169a71a9a93ac65deef8a4a8406d9"
         },
         "encoding": {
          "color": {
           "value": "#7CB9E8"
          },
          "tooltip": [
           {
            "field": "Month",
            "type": "nominal"
           },
           {
            "field": "Volume",
            "type": "quantitative"
           }
          ],
          "x": {
           "field": "Month",
           "sort": [
            "Jan",
            "Feb",
            "Mar",
            "Apr",
            "May",
            "Jun",
            "Jul",
            "Aug",
            "Sep",
            "Oct",
            "Nov",
            "Dec"
           ],
           "type": "nominal"
          },
          "y": {
           "field": "Volume",
           "type": "quantitative"
          }
         },
         "height": 300,
         "mark": "bar",
         "title": "Crime Volume by Month",
         "width": 800
        }
       ]
      },
      "text/plain": [
       "<VegaLite 4 object>\n",
       "\n",
       "If you see this message, it means the renderer has not been properly enabled\n",
       "for the frontend that you are using. For more information, see\n",
       "https://altair-viz.github.io/user_guide/troubleshooting.html\n"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "time&seasonal"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "id": "45a80201",
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "application/vnd.vegalite.v4+json": {
       "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json",
       "config": {
        "view": {
         "continuousHeight": 300,
         "continuousWidth": 400
        }
       },
       "datasets": {
        "data-956b170b94176af42f416598505d79b6": [
         {
          "UCR Literal": "LARCENY-FROM VEHICLE",
          "Volume": 127901
         },
         {
          "UCR Literal": "LARCENY-NON VEHICLE",
          "Volume": 95293
         },
         {
          "UCR Literal": "AUTO THEFT",
          "Volume": 55458
         },
         {
          "UCR Literal": "BURGLARY-RESIDENCE",
          "Volume": 49576
         },
         {
          "UCR Literal": "AGG ASSAULT",
          "Volume": 31417
         },
         {
          "UCR Literal": "ROBBERY-PEDESTRIAN",
          "Volume": 17164
         },
         {
          "UCR Literal": "BURGLARY-NONRES",
          "Volume": 10950
         },
         {
          "UCR Literal": "BURGLARY",
          "Volume": 4930
         },
         {
          "UCR Literal": "ROBBERY-COMMERCIAL",
          "Volume": 2317
         },
         {
          "UCR Literal": "ROBBERY-RESIDENCE",
          "Volume": 2230
         },
         {
          "UCR Literal": "ROBBERY",
          "Volume": 2188
         },
         {
          "UCR Literal": "HOMICIDE",
          "Volume": 1384
         }
        ],
        "data-b17d08c1f1ee63371d869e49f3ea2818": [
         {
          "Neighborhood": "Downtown",
          "Volume": 30330
         },
         {
          "Neighborhood": "Midtown",
          "Volume": 22517
         },
         {
          "Neighborhood": "Old Fourth Ward",
          "Volume": 11417
         },
         {
          "Neighborhood": "West End",
          "Volume": 9693
         },
         {
          "Neighborhood": "Lenox",
          "Volume": 6934
         },
         {
          "Neighborhood": "North Buckhead",
          "Volume": 6666
         },
         {
          "Neighborhood": "Lindbergh/Morosgo",
          "Volume": 6369
         },
         {
          "Neighborhood": "Vine City",
          "Volume": 6328
         },
         {
          "Neighborhood": "Greenbriar",
          "Volume": 6152
         },
         {
          "Neighborhood": "Grant Park",
          "Volume": 6120
         },
         {
          "Neighborhood": "Sylvan Hills",
          "Volume": 6085
         },
         {
          "Neighborhood": "Mechanicsville",
          "Volume": 5830
         },
         {
          "Neighborhood": "Edgewood",
          "Volume": 5790
         },
         {
          "Neighborhood": "Grove Park",
          "Volume": 5764
         },
         {
          "Neighborhood": "Home Park",
          "Volume": 5680
         },
         {
          "Neighborhood": "Pittsburgh",
          "Volume": 5673
         },
         {
          "Neighborhood": "Berkeley Park",
          "Volume": 5258
         },
         {
          "Neighborhood": "Inman Park",
          "Volume": 5000
         },
         {
          "Neighborhood": "English Avenue",
          "Volume": 4961
         },
         {
          "Neighborhood": "East Atlanta",
          "Volume": 4547
         }
        ]
       },
       "hconcat": [
        {
         "data": {
          "name": "data-b17d08c1f1ee63371d869e49f3ea2818"
         },
         "encoding": {
          "x": {
           "field": "Volume",
           "type": "quantitative"
          },
          "y": {
           "field": "Neighborhood",
           "sort": "x",
           "type": "nominal"
          }
         },
         "height": 500,
         "mark": "bar",
         "title": "Most Dangerous Neighborhoods",
         "width": 400
        },
        {
         "data": {
          "name": "data-956b170b94176af42f416598505d79b6"
         },
         "encoding": {
          "x": {
           "field": "UCR Literal",
           "sort": "-y",
           "type": "nominal"
          },
          "y": {
           "field": "Volume",
           "type": "quantitative"
          }
         },
         "height": 500,
         "mark": "bar",
         "title": "Top 10 Common Offenses",
         "width": 300
        }
       ]
      },
      "text/plain": [
       "<VegaLite 4 object>\n",
       "\n",
       "If you see this message, it means the renderer has not been properly enabled\n",
       "for the frontend that you are using. For more information, see\n",
       "https://altair-viz.github.io/user_guide/troubleshooting.html\n"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "neighborhood|to"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "id": "f4d0917e",
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "application/vnd.vegalite.v4+json": {
       "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json",
       "config": {
        "view": {
         "continuousHeight": 300,
         "continuousWidth": 400
        }
       },
       "data": {
        "name": "data-4dcf4e38b786574dbf787b6948d1041f"
       },
       "datasets": {
        "data-4dcf4e38b786574dbf787b6948d1041f": [
         {
          "Hour": "01",
          "Month": "Jan",
          "Volume": 1030
         },
         {
          "Hour": "02",
          "Month": "Jan",
          "Volume": 797
         },
         {
          "Hour": "03",
          "Month": "Jan",
          "Volume": 605
         },
         {
          "Hour": "04",
          "Month": "Jan",
          "Volume": 467
         },
         {
          "Hour": "05",
          "Month": "Jan",
          "Volume": 427
         },
         {
          "Hour": "06",
          "Month": "Jan",
          "Volume": 565
         },
         {
          "Hour": "07",
          "Month": "Jan",
          "Volume": 804
         },
         {
          "Hour": "08",
          "Month": "Jan",
          "Volume": 1350
         },
         {
          "Hour": "09",
          "Month": "Jan",
          "Volume": 1082
         },
         {
          "Hour": "10",
          "Month": "Jan",
          "Volume": 1161
         },
         {
          "Hour": "11",
          "Month": "Jan",
          "Volume": 1231
         },
         {
          "Hour": "12",
          "Month": "Jan",
          "Volume": 1926
         },
         {
          "Hour": "13",
          "Month": "Jan",
          "Volume": 1438
         },
         {
          "Hour": "14",
          "Month": "Jan",
          "Volume": 1526
         },
         {
          "Hour": "15",
          "Month": "Jan",
          "Volume": 1737
         },
         {
          "Hour": "16",
          "Month": "Jan",
          "Volume": 1747
         },
         {
          "Hour": "17",
          "Month": "Jan",
          "Volume": 2001
         },
         {
          "Hour": "18",
          "Month": "Jan",
          "Volume": 2185
         },
         {
          "Hour": "19",
          "Month": "Jan",
          "Volume": 2317
         },
         {
          "Hour": "20",
          "Month": "Jan",
          "Volume": 2048
         },
         {
          "Hour": "21",
          "Month": "Jan",
          "Volume": 1823
         },
         {
          "Hour": "22",
          "Month": "Jan",
          "Volume": 1805
         },
         {
          "Hour": "23",
          "Month": "Jan",
          "Volume": 1537
         },
         {
          "Hour": "24",
          "Month": "Jan",
          "Volume": 1635
         },
         {
          "Hour": "01",
          "Month": "Feb",
          "Volume": 808
         },
         {
          "Hour": "02",
          "Month": "Feb",
          "Volume": 617
         },
         {
          "Hour": "03",
          "Month": "Feb",
          "Volume": 504
         },
         {
          "Hour": "04",
          "Month": "Feb",
          "Volume": 388
         },
         {
          "Hour": "05",
          "Month": "Feb",
          "Volume": 373
         },
         {
          "Hour": "06",
          "Month": "Feb",
          "Volume": 444
         },
         {
          "Hour": "07",
          "Month": "Feb",
          "Volume": 600
         },
         {
          "Hour": "08",
          "Month": "Feb",
          "Volume": 1110
         },
         {
          "Hour": "09",
          "Month": "Feb",
          "Volume": 912
         },
         {
          "Hour": "10",
          "Month": "Feb",
          "Volume": 1014
         },
         {
          "Hour": "11",
          "Month": "Feb",
          "Volume": 1066
         },
         {
          "Hour": "12",
          "Month": "Feb",
          "Volume": 1573
         },
         {
          "Hour": "13",
          "Month": "Feb",
          "Volume": 1224
         },
         {
          "Hour": "14",
          "Month": "Feb",
          "Volume": 1229
         },
         {
          "Hour": "15",
          "Month": "Feb",
          "Volume": 1471
         },
         {
          "Hour": "16",
          "Month": "Feb",
          "Volume": 1452
         },
         {
          "Hour": "17",
          "Month": "Feb",
          "Volume": 1592
         },
         {
          "Hour": "18",
          "Month": "Feb",
          "Volume": 1707
         },
         {
          "Hour": "19",
          "Month": "Feb",
          "Volume": 1766
         },
         {
          "Hour": "20",
          "Month": "Feb",
          "Volume": 1681
         },
         {
          "Hour": "21",
          "Month": "Feb",
          "Volume": 1433
         },
         {
          "Hour": "22",
          "Month": "Feb",
          "Volume": 1384
         },
         {
          "Hour": "23",
          "Month": "Feb",
          "Volume": 1289
         },
         {
          "Hour": "24",
          "Month": "Feb",
          "Volume": 1301
         },
         {
          "Hour": "01",
          "Month": "Mar",
          "Volume": 895
         },
         {
          "Hour": "02",
          "Month": "Mar",
          "Volume": 699
         },
         {
          "Hour": "03",
          "Month": "Mar",
          "Volume": 610
         },
         {
          "Hour": "04",
          "Month": "Mar",
          "Volume": 453
         },
         {
          "Hour": "05",
          "Month": "Mar",
          "Volume": 429
         },
         {
          "Hour": "06",
          "Month": "Mar",
          "Volume": 464
         },
         {
          "Hour": "07",
          "Month": "Mar",
          "Volume": 743
         },
         {
          "Hour": "08",
          "Month": "Mar",
          "Volume": 1146
         },
         {
          "Hour": "09",
          "Month": "Mar",
          "Volume": 988
         },
         {
          "Hour": "10",
          "Month": "Mar",
          "Volume": 1135
         },
         {
          "Hour": "11",
          "Month": "Mar",
          "Volume": 1191
         },
         {
          "Hour": "12",
          "Month": "Mar",
          "Volume": 1763
         },
         {
          "Hour": "13",
          "Month": "Mar",
          "Volume": 1310
         },
         {
          "Hour": "14",
          "Month": "Mar",
          "Volume": 1379
         },
         {
          "Hour": "15",
          "Month": "Mar",
          "Volume": 1561
         },
         {
          "Hour": "16",
          "Month": "Mar",
          "Volume": 1557
         },
         {
          "Hour": "17",
          "Month": "Mar",
          "Volume": 1708
         },
         {
          "Hour": "18",
          "Month": "Mar",
          "Volume": 1885
         },
         {
          "Hour": "19",
          "Month": "Mar",
          "Volume": 1721
         },
         {
          "Hour": "20",
          "Month": "Mar",
          "Volume": 1813
         },
         {
          "Hour": "21",
          "Month": "Mar",
          "Volume": 1675
         },
         {
          "Hour": "22",
          "Month": "Mar",
          "Volume": 1614
         },
         {
          "Hour": "23",
          "Month": "Mar",
          "Volume": 1521
         },
         {
          "Hour": "24",
          "Month": "Mar",
          "Volume": 1520
         },
         {
          "Hour": "01",
          "Month": "Apr",
          "Volume": 990
         },
         {
          "Hour": "02",
          "Month": "Apr",
          "Volume": 846
         },
         {
          "Hour": "03",
          "Month": "Apr",
          "Volume": 620
         },
         {
          "Hour": "04",
          "Month": "Apr",
          "Volume": 473
         },
         {
          "Hour": "05",
          "Month": "Apr",
          "Volume": 417
         },
         {
          "Hour": "06",
          "Month": "Apr",
          "Volume": 498
         },
         {
          "Hour": "07",
          "Month": "Apr",
          "Volume": 713
         },
         {
          "Hour": "08",
          "Month": "Apr",
          "Volume": 1246
         },
         {
          "Hour": "09",
          "Month": "Apr",
          "Volume": 999
         },
         {
          "Hour": "10",
          "Month": "Apr",
          "Volume": 1139
         },
         {
          "Hour": "11",
          "Month": "Apr",
          "Volume": 1268
         },
         {
          "Hour": "12",
          "Month": "Apr",
          "Volume": 1814
         },
         {
          "Hour": "13",
          "Month": "Apr",
          "Volume": 1420
         },
         {
          "Hour": "14",
          "Month": "Apr",
          "Volume": 1537
         },
         {
          "Hour": "15",
          "Month": "Apr",
          "Volume": 1613
         },
         {
          "Hour": "16",
          "Month": "Apr",
          "Volume": 1675
         },
         {
          "Hour": "17",
          "Month": "Apr",
          "Volume": 1799
         },
         {
          "Hour": "18",
          "Month": "Apr",
          "Volume": 1943
         },
         {
          "Hour": "19",
          "Month": "Apr",
          "Volume": 1866
         },
         {
          "Hour": "20",
          "Month": "Apr",
          "Volume": 1737
         },
         {
          "Hour": "21",
          "Month": "Apr",
          "Volume": 1763
         },
         {
          "Hour": "22",
          "Month": "Apr",
          "Volume": 1824
         },
         {
          "Hour": "23",
          "Month": "Apr",
          "Volume": 1570
         },
         {
          "Hour": "24",
          "Month": "Apr",
          "Volume": 1614
         },
         {
          "Hour": "01",
          "Month": "May",
          "Volume": 1188
         },
         {
          "Hour": "02",
          "Month": "May",
          "Volume": 918
         },
         {
          "Hour": "03",
          "Month": "May",
          "Volume": 770
         },
         {
          "Hour": "04",
          "Month": "May",
          "Volume": 612
         },
         {
          "Hour": "05",
          "Month": "May",
          "Volume": 472
         },
         {
          "Hour": "06",
          "Month": "May",
          "Volume": 549
         },
         {
          "Hour": "07",
          "Month": "May",
          "Volume": 813
         },
         {
          "Hour": "08",
          "Month": "May",
          "Volume": 1321
         },
         {
          "Hour": "09",
          "Month": "May",
          "Volume": 1152
         },
         {
          "Hour": "10",
          "Month": "May",
          "Volume": 1291
         },
         {
          "Hour": "11",
          "Month": "May",
          "Volume": 1416
         },
         {
          "Hour": "12",
          "Month": "May",
          "Volume": 2112
         },
         {
          "Hour": "13",
          "Month": "May",
          "Volume": 1509
         },
         {
          "Hour": "14",
          "Month": "May",
          "Volume": 1691
         },
         {
          "Hour": "15",
          "Month": "May",
          "Volume": 1835
         },
         {
          "Hour": "16",
          "Month": "May",
          "Volume": 1819
         },
         {
          "Hour": "17",
          "Month": "May",
          "Volume": 2016
         },
         {
          "Hour": "18",
          "Month": "May",
          "Volume": 1986
         },
         {
          "Hour": "19",
          "Month": "May",
          "Volume": 2005
         },
         {
          "Hour": "20",
          "Month": "May",
          "Volume": 1969
         },
         {
          "Hour": "21",
          "Month": "May",
          "Volume": 1811
         },
         {
          "Hour": "22",
          "Month": "May",
          "Volume": 1980
         },
         {
          "Hour": "23",
          "Month": "May",
          "Volume": 1881
         },
         {
          "Hour": "24",
          "Month": "May",
          "Volume": 1885
         },
         {
          "Hour": "01",
          "Month": "Jun",
          "Volume": 1291
         },
         {
          "Hour": "02",
          "Month": "Jun",
          "Volume": 979
         },
         {
          "Hour": "03",
          "Month": "Jun",
          "Volume": 808
         },
         {
          "Hour": "04",
          "Month": "Jun",
          "Volume": 632
         },
         {
          "Hour": "05",
          "Month": "Jun",
          "Volume": 523
         },
         {
          "Hour": "06",
          "Month": "Jun",
          "Volume": 596
         },
         {
          "Hour": "07",
          "Month": "Jun",
          "Volume": 680
         },
         {
          "Hour": "08",
          "Month": "Jun",
          "Volume": 1392
         },
         {
          "Hour": "09",
          "Month": "Jun",
          "Volume": 1123
         },
         {
          "Hour": "10",
          "Month": "Jun",
          "Volume": 1296
         },
         {
          "Hour": "11",
          "Month": "Jun",
          "Volume": 1316
         },
         {
          "Hour": "12",
          "Month": "Jun",
          "Volume": 2125
         },
         {
          "Hour": "13",
          "Month": "Jun",
          "Volume": 1504
         },
         {
          "Hour": "14",
          "Month": "Jun",
          "Volume": 1666
         },
         {
          "Hour": "15",
          "Month": "Jun",
          "Volume": 1785
         },
         {
          "Hour": "16",
          "Month": "Jun",
          "Volume": 1784
         },
         {
          "Hour": "17",
          "Month": "Jun",
          "Volume": 2008
         },
         {
          "Hour": "18",
          "Month": "Jun",
          "Volume": 2141
         },
         {
          "Hour": "19",
          "Month": "Jun",
          "Volume": 1872
         },
         {
          "Hour": "20",
          "Month": "Jun",
          "Volume": 1956
         },
         {
          "Hour": "21",
          "Month": "Jun",
          "Volume": 1796
         },
         {
          "Hour": "22",
          "Month": "Jun",
          "Volume": 2049
         },
         {
          "Hour": "23",
          "Month": "Jun",
          "Volume": 1954
         },
         {
          "Hour": "24",
          "Month": "Jun",
          "Volume": 2025
         },
         {
          "Hour": "01",
          "Month": "Jul",
          "Volume": 1420
         },
         {
          "Hour": "02",
          "Month": "Jul",
          "Volume": 1006
         },
         {
          "Hour": "03",
          "Month": "Jul",
          "Volume": 941
         },
         {
          "Hour": "04",
          "Month": "Jul",
          "Volume": 676
         },
         {
          "Hour": "05",
          "Month": "Jul",
          "Volume": 588
         },
         {
          "Hour": "06",
          "Month": "Jul",
          "Volume": 579
         },
         {
          "Hour": "07",
          "Month": "Jul",
          "Volume": 761
         },
         {
          "Hour": "08",
          "Month": "Jul",
          "Volume": 1357
         },
         {
          "Hour": "09",
          "Month": "Jul",
          "Volume": 1117
         },
         {
          "Hour": "10",
          "Month": "Jul",
          "Volume": 1314
         },
         {
          "Hour": "11",
          "Month": "Jul",
          "Volume": 1361
         },
         {
          "Hour": "12",
          "Month": "Jul",
          "Volume": 2164
         },
         {
          "Hour": "13",
          "Month": "Jul",
          "Volume": 1612
         },
         {
          "Hour": "14",
          "Month": "Jul",
          "Volume": 1767
         },
         {
          "Hour": "15",
          "Month": "Jul",
          "Volume": 1910
         },
         {
          "Hour": "16",
          "Month": "Jul",
          "Volume": 1915
         },
         {
          "Hour": "17",
          "Month": "Jul",
          "Volume": 2049
         },
         {
          "Hour": "18",
          "Month": "Jul",
          "Volume": 2262
         },
         {
          "Hour": "19",
          "Month": "Jul",
          "Volume": 2079
         },
         {
          "Hour": "20",
          "Month": "Jul",
          "Volume": 2082
         },
         {
          "Hour": "21",
          "Month": "Jul",
          "Volume": 1980
         },
         {
          "Hour": "22",
          "Month": "Jul",
          "Volume": 2087
         },
         {
          "Hour": "23",
          "Month": "Jul",
          "Volume": 2064
         },
         {
          "Hour": "24",
          "Month": "Jul",
          "Volume": 2112
         },
         {
          "Hour": "01",
          "Month": "Aug",
          "Volume": 1191
         },
         {
          "Hour": "02",
          "Month": "Aug",
          "Volume": 943
         },
         {
          "Hour": "03",
          "Month": "Aug",
          "Volume": 852
         },
         {
          "Hour": "04",
          "Month": "Aug",
          "Volume": 607
         },
         {
          "Hour": "05",
          "Month": "Aug",
          "Volume": 542
         },
         {
          "Hour": "06",
          "Month": "Aug",
          "Volume": 661
         },
         {
          "Hour": "07",
          "Month": "Aug",
          "Volume": 845
         },
         {
          "Hour": "08",
          "Month": "Aug",
          "Volume": 1498
         },
         {
          "Hour": "09",
          "Month": "Aug",
          "Volume": 1268
         },
         {
          "Hour": "10",
          "Month": "Aug",
          "Volume": 1289
         },
         {
          "Hour": "11",
          "Month": "Aug",
          "Volume": 1433
         },
         {
          "Hour": "12",
          "Month": "Aug",
          "Volume": 2199
         },
         {
          "Hour": "13",
          "Month": "Aug",
          "Volume": 1508
         },
         {
          "Hour": "14",
          "Month": "Aug",
          "Volume": 1640
         },
         {
          "Hour": "15",
          "Month": "Aug",
          "Volume": 1859
         },
         {
          "Hour": "16",
          "Month": "Aug",
          "Volume": 1801
         },
         {
          "Hour": "17",
          "Month": "Aug",
          "Volume": 2048
         },
         {
          "Hour": "18",
          "Month": "Aug",
          "Volume": 2126
         },
         {
          "Hour": "19",
          "Month": "Aug",
          "Volume": 2010
         },
         {
          "Hour": "20",
          "Month": "Aug",
          "Volume": 2040
         },
         {
          "Hour": "21",
          "Month": "Aug",
          "Volume": 1983
         },
         {
          "Hour": "22",
          "Month": "Aug",
          "Volume": 2080
         },
         {
          "Hour": "23",
          "Month": "Aug",
          "Volume": 1844
         },
         {
          "Hour": "24",
          "Month": "Aug",
          "Volume": 2081
         },
         {
          "Hour": "01",
          "Month": "Sep",
          "Volume": 1072
         },
         {
          "Hour": "02",
          "Month": "Sep",
          "Volume": 914
         },
         {
          "Hour": "03",
          "Month": "Sep",
          "Volume": 696
         },
         {
          "Hour": "04",
          "Month": "Sep",
          "Volume": 556
         },
         {
          "Hour": "05",
          "Month": "Sep",
          "Volume": 499
         },
         {
          "Hour": "06",
          "Month": "Sep",
          "Volume": 596
         },
         {
          "Hour": "07",
          "Month": "Sep",
          "Volume": 821
         },
         {
          "Hour": "08",
          "Month": "Sep",
          "Volume": 1297
         },
         {
          "Hour": "09",
          "Month": "Sep",
          "Volume": 1119
         },
         {
          "Hour": "10",
          "Month": "Sep",
          "Volume": 1243
         },
         {
          "Hour": "11",
          "Month": "Sep",
          "Volume": 1300
         },
         {
          "Hour": "12",
          "Month": "Sep",
          "Volume": 1977
         },
         {
          "Hour": "13",
          "Month": "Sep",
          "Volume": 1436
         },
         {
          "Hour": "14",
          "Month": "Sep",
          "Volume": 1498
         },
         {
          "Hour": "15",
          "Month": "Sep",
          "Volume": 1712
         },
         {
          "Hour": "16",
          "Month": "Sep",
          "Volume": 1619
         },
         {
          "Hour": "17",
          "Month": "Sep",
          "Volume": 1887
         },
         {
          "Hour": "18",
          "Month": "Sep",
          "Volume": 1984
         },
         {
          "Hour": "19",
          "Month": "Sep",
          "Volume": 1883
         },
         {
          "Hour": "20",
          "Month": "Sep",
          "Volume": 1919
         },
         {
          "Hour": "21",
          "Month": "Sep",
          "Volume": 1824
         },
         {
          "Hour": "22",
          "Month": "Sep",
          "Volume": 1828
         },
         {
          "Hour": "23",
          "Month": "Sep",
          "Volume": 1659
         },
         {
          "Hour": "24",
          "Month": "Sep",
          "Volume": 1680
         },
         {
          "Hour": "01",
          "Month": "Oct",
          "Volume": 1105
         },
         {
          "Hour": "02",
          "Month": "Oct",
          "Volume": 830
         },
         {
          "Hour": "03",
          "Month": "Oct",
          "Volume": 659
         },
         {
          "Hour": "04",
          "Month": "Oct",
          "Volume": 524
         },
         {
          "Hour": "05",
          "Month": "Oct",
          "Volume": 525
         },
         {
          "Hour": "06",
          "Month": "Oct",
          "Volume": 573
         },
         {
          "Hour": "07",
          "Month": "Oct",
          "Volume": 792
         },
         {
          "Hour": "08",
          "Month": "Oct",
          "Volume": 1348
         },
         {
          "Hour": "09",
          "Month": "Oct",
          "Volume": 1141
         },
         {
          "Hour": "10",
          "Month": "Oct",
          "Volume": 1246
         },
         {
          "Hour": "11",
          "Month": "Oct",
          "Volume": 1290
         },
         {
          "Hour": "12",
          "Month": "Oct",
          "Volume": 1958
         },
         {
          "Hour": "13",
          "Month": "Oct",
          "Volume": 1503
         },
         {
          "Hour": "14",
          "Month": "Oct",
          "Volume": 1560
         },
         {
          "Hour": "15",
          "Month": "Oct",
          "Volume": 1753
         },
         {
          "Hour": "16",
          "Month": "Oct",
          "Volume": 1778
         },
         {
          "Hour": "17",
          "Month": "Oct",
          "Volume": 1916
         },
         {
          "Hour": "18",
          "Month": "Oct",
          "Volume": 2108
         },
         {
          "Hour": "19",
          "Month": "Oct",
          "Volume": 2116
         },
         {
          "Hour": "20",
          "Month": "Oct",
          "Volume": 2198
         },
         {
          "Hour": "21",
          "Month": "Oct",
          "Volume": 1950
         },
         {
          "Hour": "22",
          "Month": "Oct",
          "Volume": 1916
         },
         {
          "Hour": "23",
          "Month": "Oct",
          "Volume": 1710
         },
         {
          "Hour": "24",
          "Month": "Oct",
          "Volume": 1826
         },
         {
          "Hour": "01",
          "Month": "Nov",
          "Volume": 969
         },
         {
          "Hour": "02",
          "Month": "Nov",
          "Volume": 689
         },
         {
          "Hour": "03",
          "Month": "Nov",
          "Volume": 609
         },
         {
          "Hour": "04",
          "Month": "Nov",
          "Volume": 431
         },
         {
          "Hour": "05",
          "Month": "Nov",
          "Volume": 439
         },
         {
          "Hour": "06",
          "Month": "Nov",
          "Volume": 553
         },
         {
          "Hour": "07",
          "Month": "Nov",
          "Volume": 752
         },
         {
          "Hour": "08",
          "Month": "Nov",
          "Volume": 1374
         },
         {
          "Hour": "09",
          "Month": "Nov",
          "Volume": 1119
         },
         {
          "Hour": "10",
          "Month": "Nov",
          "Volume": 1270
         },
         {
          "Hour": "11",
          "Month": "Nov",
          "Volume": 1342
         },
         {
          "Hour": "12",
          "Month": "Nov",
          "Volume": 1950
         },
         {
          "Hour": "13",
          "Month": "Nov",
          "Volume": 1451
         },
         {
          "Hour": "14",
          "Month": "Nov",
          "Volume": 1573
         },
         {
          "Hour": "15",
          "Month": "Nov",
          "Volume": 1733
         },
         {
          "Hour": "16",
          "Month": "Nov",
          "Volume": 1661
         },
         {
          "Hour": "17",
          "Month": "Nov",
          "Volume": 1999
         },
         {
          "Hour": "18",
          "Month": "Nov",
          "Volume": 2248
         },
         {
          "Hour": "19",
          "Month": "Nov",
          "Volume": 2149
         },
         {
          "Hour": "20",
          "Month": "Nov",
          "Volume": 2010
         },
         {
          "Hour": "21",
          "Month": "Nov",
          "Volume": 1745
         },
         {
          "Hour": "22",
          "Month": "Nov",
          "Volume": 1689
         },
         {
          "Hour": "23",
          "Month": "Nov",
          "Volume": 1469
         },
         {
          "Hour": "24",
          "Month": "Nov",
          "Volume": 1588
         },
         {
          "Hour": "01",
          "Month": "Dec",
          "Volume": 963
         },
         {
          "Hour": "02",
          "Month": "Dec",
          "Volume": 688
         },
         {
          "Hour": "03",
          "Month": "Dec",
          "Volume": 639
         },
         {
          "Hour": "04",
          "Month": "Dec",
          "Volume": 444
         },
         {
          "Hour": "05",
          "Month": "Dec",
          "Volume": 423
         },
         {
          "Hour": "06",
          "Month": "Dec",
          "Volume": 574
         },
         {
          "Hour": "07",
          "Month": "Dec",
          "Volume": 745
         },
         {
          "Hour": "08",
          "Month": "Dec",
          "Volume": 1285
         },
         {
          "Hour": "09",
          "Month": "Dec",
          "Volume": 1083
         },
         {
          "Hour": "10",
          "Month": "Dec",
          "Volume": 1270
         },
         {
          "Hour": "11",
          "Month": "Dec",
          "Volume": 1251
         },
         {
          "Hour": "12",
          "Month": "Dec",
          "Volume": 2101
         },
         {
          "Hour": "13",
          "Month": "Dec",
          "Volume": 1524
         },
         {
          "Hour": "14",
          "Month": "Dec",
          "Volume": 1627
         },
         {
          "Hour": "15",
          "Month": "Dec",
          "Volume": 1756
         },
         {
          "Hour": "16",
          "Month": "Dec",
          "Volume": 1758
         },
         {
          "Hour": "17",
          "Month": "Dec",
          "Volume": 2109
         },
         {
          "Hour": "18",
          "Month": "Dec",
          "Volume": 2335
         },
         {
          "Hour": "19",
          "Month": "Dec",
          "Volume": 2349
         },
         {
          "Hour": "20",
          "Month": "Dec",
          "Volume": 2110
         },
         {
          "Hour": "21",
          "Month": "Dec",
          "Volume": 1845
         },
         {
          "Hour": "22",
          "Month": "Dec",
          "Volume": 1758
         },
         {
          "Hour": "23",
          "Month": "Dec",
          "Volume": 1537
         },
         {
          "Hour": "24",
          "Month": "Dec",
          "Volume": 1598
         }
        ]
       },
       "encoding": {
        "color": {
         "field": "Volume",
         "type": "quantitative"
        },
        "tooltip": [
         {
          "field": "Month",
          "type": "nominal"
         },
         {
          "field": "Hour",
          "type": "nominal"
         },
         {
          "field": "Volume",
          "type": "quantitative"
         }
        ],
        "x": {
         "field": "Hour",
         "type": "nominal"
        },
        "y": {
         "field": "Month",
         "sort": [
          1,
          2,
          3,
          4,
          5,
          6,
          7,
          8,
          9,
          10,
          11,
          12
         ],
         "type": "nominal"
        }
       },
       "height": 500,
       "mark": "rect",
       "title": "Crime Volume by Hour/Month",
       "width": 500
      },
      "text/plain": [
       "<VegaLite 4 object>\n",
       "\n",
       "If you see this message, it means the renderer has not been properly enabled\n",
       "for the frontend that you are using. For more information, see\n",
       "https://altair-viz.github.io/user_guide/troubleshooting.html\n"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "heat "
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3.9.7 ('base')",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.9.7"
  },
  "vscode": {
   "interpreter": {
    "hash": "d86d92ee7acdc728181fa9c30818268f84bb4c77506e64db43eff0c6fd2d54c5"
   }
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
