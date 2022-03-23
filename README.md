Adatbázis cucc elindítása:

cmd---> mongod

c-n létrehozni data mappát majd benne a db mappát

mongodbcompass mongodb://127.0.0.1:27017 Végig nyitva hagyni !!!!!!!!!!!!!

létrehozni adatbázis "hahu" néven majd kollekciókat és utána beimportálni az adatokat

új mappa és a saját névvel

terminált nyitni és cmd kell

express --no-view "név" -------------- ha nem jó npm i -g express-generator

models mappa létrehozni --- >>>> benne hirdetes.js és kategoria.js

előtte cd hahuapi

((npm i --save mongoose, npm i --save-dev nodemon))

ezután package.json startnál nodemon

app.js be kell copyzni ezeket

var mongoose = require('mongoose')
const MONGODB_URI = 'mongodb://127.0.0.1:27017'

mongoose
    .connect(MONGODB_URI, {useNewUrlParser: true})
    .then(console.log("Connected to MongoDB"))
    .catch(err => {
        console.log(err)
    })


kategoria.js

const mongoose = require('mongoose')

const Schema = mongoose.Schema;

const kategoriaSchema = new Schema({
    _id: Number,
    nev: {
        type: String,
        required: true,
        unique: true,
        maxlength: 30
    }
})

module.exports = mongoose.model('Kategoria', kategoriaSchema, 'kategoriak')

hirdetes.js

const mongoose = require('mongoose')

const Schema = mongoose.Schema;

const hirdetesSchema = new Schema({
    _id: Number,
    kategoria: {
        type: Number,
        default: 1
    },
    cim: {
        type: String,
        required: true,
        unique: true,
        maxlength: 100
    },
    leiras: {
        type: String,
        maxlength: 3000
    },
    hirdetesDatuma: {
        type: Date,
        default: Date.now
    },
    serulesmentes: Boolean,
    arFt: {
        type: Number,
        required: true
    },
    kepUrl: {
        type: String,
        maxlength: 100
    }
})

module.exports = mongoose.model('Hirdetes', hirdetesSchema, 'hirdetesek')

új fájl a routesba ahol elérési útat fogunk csinálni

útvonalt regisztrálni az app.jsbe

var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var hahuRouter = require('./routes/hahu');

var mongoose = require('mongoose')
const MONGODB_URI = 'mongodb://127.0.0.1:27017/hahu'

mongoose
    .connect(MONGODB_URI, {useNewUrlParser: true})
    .then(console.log("Connected to MongoDB"))
    .catch(err => {
        console.log(err)
    })

var app = express();

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/api/hahu', hahuRouter);

module.exports = app;


hahu.js a routes-ba

var express = require('express');
var router = express.Router();

var Hirdetes = require('../models/hirdetes')

/* GET home page. */
router.post('/', function(req, res, next) {
    const _id = req.body._id;
    const kategoria = req.body.kategoria;
    const cim = req.body.cim;
    const leiras = req.body.leiras;
    const hirdetesDatuma = req.body.hirdetesDatuma;
    const serulesmentes = req.body.serulesmentes;
    const arFt = req.body.arFt;
    const kepUrl = req.body.kepUrl;

    const hirdetes = new Hirdetes({
        _id, kategoria, cim, leiras, hirdetesDatuma, serulesmentes, arFt, kepUrl
    });
    hirdetes
    .save()
    .then(res.json({
        "message": "A rekord a rögzítése sikeres!"
    }))
    .catch(err => 
        console.log(err));
});

module.exports = router;

kérést elküldeni!!!!

thunderclient a villám szaros ikon

headersba plusszba ++++ content-type   application/json

bodyba

{
    "_id": 34,
    "kategoria": 4,
    "cim": "Traktor 79",
    "leiras": "Erős",
    "hirdetesDatuma": "2012-01-01",
    "serulesmentes": true,
    "arFt": 25000,
    "kepUrl": "https://kep.jpg"
}




2eshez!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

