#Android-多媒体相关

## 播放音效

```
private SoundPool soundPool;
soundPool = new SoundPool(10, AudioManager.STREAM_SYSTEM, 5);
int music = soundPool.load(this,R.raw.collide, 1);
soundPool.play(music, 1, 1, 0, 0, 1);
```

### Refs
[Android开发之为按钮添加音效](http://blog.csdn.net/qq435757399/article/details/8010015)  
[Android中播放声音的两种方法](http://blog.csdn.net/pku_android/article/details/7625868)
