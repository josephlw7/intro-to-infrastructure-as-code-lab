# intro-to-infrastructure-as-code-lab

provider "aws" {
  region = "us-east-1" # Change to your preferred region
}
# Security Group
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow HTTP inbound traffic"
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
    Name = "WebSecurityGroup"
  }
}

# Elastic IP
resource "aws_eip" "web_eip" {
  instance = aws_instance.web_server.id
}
# EC2 Instance
resource "aws_instance" "web_server" {
  ami             = "ami-0c55b159cbfafe1f0"
  instance_type   = "t2.micro"
  security_groups = [aws_security_group.web_sg.name]
  tags = {
    Name        = "WebServer"
    Environment = "Production"
  }
}

