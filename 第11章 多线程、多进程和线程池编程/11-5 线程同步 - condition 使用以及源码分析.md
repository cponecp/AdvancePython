from threading import Condition, Thread


class XiaoAi(Thread):
	def __init__(self, cond):
		super().__init__(name="小爱")
		self.cond = cond
	
	def run(self) -> None:
		with self.cond:
			self.cond.wait()
			print(f"{self.name}:我在")
			self.cond.notify()
			self.cond.wait()
			print(f"{self.name}:好的")


class TianMao(Thread):
	def __init__(self, cond):
		super().__init__(name="天猫")
		self.cond = cond
	
	def run(self) -> None:
		with self.cond:
			self.cond.notify()
			print(f"{self.name}:小爱同学")
			self.cond.wait()
			print(f"{self.name}:读一首诗吧")
			self.cond.notify()
			
if __name__ == '__main__':
	cond = Condition()
	xiao_ai = XiaoAi(cond)
	tian_mao =TianMao(cond)
	
	#谁先wait谁先启动，不然notify后对象还没启动
	xiao_ai.start()
	tian_mao.start()
	#Condition有两层锁，一把底层锁会在线程调用wait方法的时候释放，
	#另一把会在每次调用wait的时候分配一把并放到cond的队列中等待notify方法的唤醒
