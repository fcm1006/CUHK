import numpy as np
import matplotlib.pyplot as plt

# Bus Stops Coordinates(28 bus stops)
coordinates = np.array([[114.209426016536,22.4137903494608, 20.06899],[114.210192043735,22.4144711962683, 4.201964],
                        [114.208212846443,22.4155295916009, 19.911699],[114.210815799947,22.4159699669869, 8.26567],
                        [114.209875797763,22.4179990396123, 19.841451],[114.21046935965,22.4178151221997, 9.75248],
                        [114.212967831967,22.4190120770855,11.067285],[114.212886628271,22.4192848786926,7.88124],
                        [114.205321799371,22.4188029980632, 17.276491],[114.212119845464,22.4203884516352, 17.138578],
                        [114.207405161929,22.4198603629261, 14.3359],[114.207191985521,22.4198668888052, 9.142796],
                        [114.203085882698,22.4198442267913, 15.481109],[114.207462427955,22.4210971104035, 22.663655],
                        [114.207473156783,22.4213400962089, 23.312776],[114.205506984131,22.4204147423153, 16.188751],
                        [114.205323223529,22.4202973450688, 13.554565],[114.201306860309,22.4224730453109, 13.028844],
                        [114.203518250003,22.4214229071764, 11.930646],[114.203447952144,22.4214314296142, 20.841772],
                        [114.204634397586,22.4218691517594, 20.674301],[114.205722062928,22.4230322016214, 19.799272],
                        [114.206643302875,22.4238498599693, 15.013307],[114.207565452625,22.425037256415, 3.914526],
                        [114.206721077661,22.4256069617219, 5.8678],[114.206136361142,22.425643570059, 8.320608],
                        [114.205862746727,22.42582061566, 12.894038],[114.204354370494,22.4275468634924, 5.16055]])

slope = np.array([20.06899, 4.201964, 19.911699, 8.26567, 19.841451, 9.75248, 11.067285, 7.88124, 17.276491, 17.138578, 14.3359,
         9.142796, 15.481109, 22.663655, 23.312776, 16.188751, 13.554565, 13.028844, 11.930646, 20.841772, 20.674301,
         19.799272, 15.013307, 3.914526, 5.8678, 8.320608, 12.894038, 5.16055])

def getdistmat(coordinates):
    num = coordinates.shape[0]
    distmat = np.zeros((28, 28))
    for i in range(num):
        for j in range(i, num):
            distmat[i][j] = distmat[j][i] = np.linalg.norm(
                coordinates[i] - coordinates[j])
    return distmat


# #//Initialization
distmat = getdistmat(coordinates)
numant = 45 ##// The number of ants
numcity = coordinates.shape[0] ##// The number of bus stops
alpha = 1 ##// Pheromone importance factor
beta = 5 ##// Heuristic function importance factor
rho = 0.1 ##// The rate of evaporation of pheromones
Q = 1 ##//Total pheromone release
iter = 0##//Iteration
itermax = 200#//Itermax
etatable = 1.0 / (distmat + (slope *0.001) + np.diag([1e10] * numcity))  #// The heuristic function matrix, which represents the expected degree of ants moving from stop i to stop j
pheromonetable = np.ones((numcity, numcity)) #// Pheromone matrix
pathtable = np.zeros((numant, numcity)).astype(int) #// Path table
distmat = getdistmat(coordinates) #// Distance matrix

lengthaver = np.zeros(itermax) #// Average length of paths across iteration
lengthbest = np.zeros(itermax) #// Optimal path lengths encountered for each iteration and its predecessors
pathbest = np.zeros((itermax, numcity)) #// Best path lengths encountered for each generation and its predecessors
#//核心点-循环迭代
while iter < itermax:
    #// 随机产生各个蚂蚁的起点城市
    if numant <= numcity:
        #// 城市数比蚂蚁数多
        pathtable[:, 0] = np.random.permutation(range(0, numcity))[:numant]
    else:
        #// 蚂蚁数比城市数多，需要补足
        pathtable[:numcity, 0] = np.random.permutation(range(0, numcity))[:]
        pathtable[numcity:, 0] = np.random.permutation(range(0, numcity))[
            :numant - numcity]
    length = np.zeros(numant)  # 计算各个蚂蚁的路径距离
    for i in range(numant):
        visiting = pathtable[i, 0]  # 当前所在的城市
        unvisited = set(range(numcity))  # 未访问的城市,以集合的形式存储{}
        unvisited.remove(visiting)  # 删除元素；利用集合的remove方法删除存储的数据内容
        for j in range(1, numcity):  # 循环numcity-1次，访问剩余的numcity-1个城市
            # 每次用轮盘法选择下一个要访问的城市
            listunvisited = list(unvisited)
            probtrans = np.zeros(len(listunvisited))
            for k in range(len(listunvisited)):
                probtrans[k] = np.power(pheromonetable[visiting][listunvisited[k]], alpha) \
                    * np.power(etatable[visiting][listunvisited[k]], beta)
            cumsumprobtrans = (probtrans / sum(probtrans)).cumsum()
            cumsumprobtrans -= np.random.rand()
            k = listunvisited[(np.where(cumsumprobtrans > 0)[0])[0]]
            # 元素的提取（也就是下一轮选的城市）
            pathtable[i, j] = k  # 添加到路径表中（也就是蚂蚁走过的路径)
            unvisited.remove(k)  # 然后在为访问城市set中remove（）删除掉该城市
            length[i] += distmat[visiting][k]
            visiting = k
        # 蚂蚁的路径距离包括最后一个城市和第一个城市的距离
        length[i] += distmat[visiting][pathtable[i, 0]]
        # 包含所有蚂蚁的一个迭代结束后，统计本次迭代的若干统计参数
    lengthaver[iter] = length.mean()
    if iter == 0:
        lengthbest[iter] = length.min()
        pathbest[iter] = pathtable[length.argmin()].copy()
    else:
        if length.min() > lengthbest[iter - 1]:
            lengthbest[iter] = lengthbest[iter - 1]
            pathbest[iter] = pathbest[iter - 1].copy()
        else:
            lengthbest[iter] = length.min()
            pathbest[iter] = pathtable[length.argmin()].copy()
    # 更新信息素
    changepheromonetable = np.zeros((numcity, numcity))
    for i in range(numant):
        for j in range(numcity - 1):
            changepheromonetable[pathtable[i, j]][pathtable[i, j + 1]] += Q / distmat[pathtable[i, j]][
                pathtable[i, j + 1]]  # 计算信息素增量
        changepheromonetable[pathtable[i, j + 1]][pathtable[i, 0]] += Q / distmat[pathtable[i, j + 1]][pathtable[i, 0]]
    pheromonetable = (1 - rho) * pheromonetable + \
        changepheromonetable  # 计算信息素公式
    if iter%30==0:
        print("iter(迭代次数):", iter)
    iter += 1  # 迭代次数指示器+1

# 做出平均路径长度和最优路径长度
fig, axes = plt.subplots(nrows=2, ncols=1, figsize=(12, 10))
axes[0].plot(lengthaver, 'k', marker=u'')
axes[0].set_title('Average Length')
axes[0].set_xlabel(u'iteration')

axes[1].plot(lengthbest, 'k', marker=u'')
axes[1].set_title('Best Length')
axes[1].set_xlabel(u'iteration')
fig.savefig('average_best.png', dpi=500, bbox_inches='tight')
plt.show()

# 作出找到的最优路径图
bestpath = pathbest[-1]
plt.plot(coordinates[:, 0], coordinates[:, 1], 'r.', marker=u'$\cdot$')
plt.xlim([114.2012, 114.2131])
plt.ylim([22.4135, 22.4279])

for i in range(numcity - 1):
    m = int(bestpath[i])
    n = int(bestpath[i + 1])
    plt.plot([coordinates[m][0], coordinates[n][0]], [
             coordinates[m][1], coordinates[n][1]], 'k')
    plt.plot([coordinates[int(bestpath[0])][0], coordinates[int(n)][0]],
             [coordinates[int(bestpath[0])][1], coordinates[int(n)][1]], 'b')

ax = plt.gca()
ax.set_title("Best Path")
ax.set_xlabel('X axis')
ax.set_ylabel('Y_axis')

plt.savefig('best path.png', dpi=500, bbox_inches='tight')
plt.show()



