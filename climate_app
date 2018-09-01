import numpy as np

import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func

from flask import Flask, jsonify

engine = create_engine("sqlite:///hawaii.sqlite")
Base = automap_base()
Base.prepare(engine, reflect=True)

Measurement = Base.classes.measurement
Station = Base.classes.station

session = Session(engine)

app = Flask(__name__)

@app.route("/")
def home():
    return (
        f"Available Routes:<br/>"
        f"/api/v1.0/precipitation<br/>"
        f"/api/v1.0/stations<br/>"
        f"/api/v1.0/tobs"
    )

@app.route("/api/v1.0/precipitation")
def precipitation():
    results = session.query(Measurement.date, Measurement.prcp).\
        filter(Measurement.date > '2016-08-23').all()
    
    precipitation = []
    for prcp in results:
        prcp_dict = {"date": "prcp"}
        prcp_dict["date"] = prcp.date
        prcp_dict["prcp"] = prcp.prcp
        precipitation.append(prcp_dict)
        
    return jsonify(precipitation)
    
@app.route("/api/v1.0/stations")
def stations():
    results = session.query(Station.station, Station.name).all()
    
    stations = list(np.ravel(results))
    
    return jsonify(stations)
    
@app.route("/api/v1.0/tobs")
def tobs():
    results = session.query(Measurement.date, Measurement.tobs).\
        filter(Measurement.date > '2016-08-23').all()
        
    temperature = []
    for tobs in results:
        tobs_dict = {"date": "tobs"}
        tobs_dict["date"] = tobs.date
        tobs_dict["tobs"] = tobs.tobs
        temperature.append(tobs_dict)
    
    return jsonify(temperature)
    
@app.route("/api/v1.0/<start>")
def start_date(start):
    results = session.query(func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs)).\
        filter(Measurement.date >= start).all()
        
    temperature = []
    for tobs in results:
        tobs_dict = {}
        tobs_dict["tmin"] = tobs[0]
        tobs_dict["tavg"] = tobs[1]
        tobs_dict["tmax"] = tobs[2]
        temperature.append(tobs_dict)
        
    return jsonify(temperature)
    
@app.route("/api/v1.0/<start>/<end>")
def calc_temps(start, end):
    results = session.query(func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs)).\
        filter(Measurement.date >= start).filter(Measurement.date <= end).all()

    temperature = []
    for tobs in results:
        tobs_dict = {}
        tobs_dict["tmin"] = tobs[0]
        tobs_dict["tavg"] = tobs[1]
        tobs_dict["tmax"] = tobs[2]
        temperature.append(tobs_dict)
        
    return jsonify(temperature)
    
if __name__ == '__main__':
    app.run(debug=True)
    