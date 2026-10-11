import pygame
import random
import sys

# Khởi tạo Pygame
pygame.init()

# Cấu hình màn hình
WIDTH, HEIGHT = 400, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Subway Surfers Python Edition")
clock = pygame.time.Clock()

# Màu sắc
WHITE = (255, 255, 255)
GRAY = (50, 50, 50)
YELLOW = (240, 200, 0)
RED = (220, 50, 50)
BLUE = (50, 120, 220)

# Cấu hình 3 làn đường (Tọa độ X)
LANES = [80, 200, 320]
current_lane = 1  # Bắt đầu ở làn giữa (Chỉ số 1)

# Nhân vật
player_x = LANES[current_lane]
player_y = 480
player_size = 40
is_jumping = False
jump_velocity = 0

# Chướng ngại vật & Điểm số
obstacles = []  # Danh sách [x, y, type]
obstacle_speed = 7
score = 0
font = pygame.font.Font(None, 36)

def spawn_obstacle():
    lane = random.choice(LANES)
    # Loại 0: Rào thấp (Cần nhảy), Loại 1: Rào cao (Cần tránh làn)
    obs_type = random.choice(["block", "barrier"]) 
    return [lane, -60, obs_type]

# Vòng lặp chính của Game
running = True
spawn_timer = 0

while running:
    clock.tick(60)
    screen.fill(GRAY)
    
    # 1. Vẽ các làn đường
    for x in [140, 260]:
        pygame.draw.line(screen, WHITE, (x, 0), (x, HEIGHT), 2)
        
    # 2. Xử lý sự kiện bàn phím
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT and current_lane > 0:
                current_lane -= 1
            elif event.key == pygame.K_RIGHT and current_lane < 2:
                current_lane += 1
            elif event.key == pygame.K_SPACE and not is_jumping:
                is_jumping = True
                jump_velocity = -15

    # Logic nhảy
    if is_jumping:
        player_y += jump_velocity
        jump_velocity += 1  # Trọng lực
        if player_y >= 480:
            player_y = 480
            is_jumping = False

    player_x = LANES[current_lane]

    # 3. Tạo và di chuyển chướng ngại vật
    spawn_timer += 1
    if spawn_timer > 45:  # Tần suất xuất hiện
        obstacles.append(spawn_obstacle())
        spawn_timer = 0

    for obs in obstacles[:]:
        obs[1] += obstacle_speed
        
        # Vẽ chướng ngại vật
        obs_rect = pygame.Rect(obs[0] - 25, obs[1], 50, 40)
        pygame.draw.rect(screen, RED, obs_rect)
        
        # Kiểm tra va chạm với người chơi
        player_rect = pygame.Rect(player_x - 20, player_y, player_size, player_size)
        if player_rect.colliderect(obs_rect):
            print(f"Game Over! Điểm số của bạn: {int(score)}")
            running = False
            
        # Xóa chướng ngại vật khi ra khỏi màn hình
        if obs[1] > HEIGHT:
            obstacles.remove(obs)
            score += 10

    # 4. Vẽ người chơi
    pygame.draw.rect(screen, BLUE, (player_x - 20, player_y, player_size, player_size))

    # 5. Hiển thị điểm số
    score_text = font.render(f"Score: {int(score)}", True, YELLOW)
    screen.blit(score_text, (10, 10))

    pygame.display.flip()

pygame.quit()
sys.exit()
