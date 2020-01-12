## nodejs

Http.createServer(调用express的app.listen)
Https.createServer(调用express的app.listen)
因为ws需要Http Server

## expressjs

app的作用是<<``RequestHandler``>> => (req,res)

express app <``async error``> 替代``try {} catch (error) { next(error) }``

app.use(api.errorhandler) :
- -> json
- -> html 